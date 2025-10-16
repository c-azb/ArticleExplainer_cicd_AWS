

# ----------- Build stage -----------
FROM public.ecr.aws/lambda/python:3.11 AS build-image

# Install basic system dependencies
RUN yum install -y make poppler-cpp-devel && yum clean all

WORKDIR /var/task
COPY requirements.txt .

# Force pip to use precompiled manylinux wheels
RUN pip install --no-cache-dir --prefer-binary -r requirements.txt --target /var/task

# ----------- Final runtime stage -----------
FROM public.ecr.aws/lambda/python:3.11

# Copy everything from build stage
COPY --from=build-image /var/task /var/task

# Copy application code
COPY . ${LAMBDA_TASK_ROOT}

# Set the handler
CMD ["main.handler"]



# # Base image for Python Lambda
# FROM public.ecr.aws/lambda/python:3.11

# # Copy only requirements first
# COPY requirements.txt .

# # Install Python dependencies
# RUN pip install --no-cache-dir -r requirements.txt

# # copy source code (excluding .dockerignore)
# #LAMBDA_TASK_ROOT AWS envirnoment variable
# COPY . ${LAMBDA_TASK_ROOT}

# # run Lambda handler
# CMD ["main.handler"]

