

# ----------- Build stage -----------
FROM public.ecr.aws/lambda/python:3.11 AS build-image

# Install basic system dependencies (no need for compilers now)
RUN yum install -y make poppler-cpp-devel && yum clean all

WORKDIR /var/task
COPY requirements.txt .

# Force pip to use precompiled manylinux wheels
RUN pip install --no-cache-dir --prefer-binary -r requirements.txt --target /var/task

# ----------- Final runtime stage -----------
FROM public.ecr.aws/lambda/python:3.11

# Copy everything from build stage
COPY --from=build-image /var/task /var/task

# Copy your application code
COPY . ${LAMBDA_TASK_ROOT}

# Set the handler (adjust to your filename and function)
CMD ["main.handler"]



# # Base image for Python Lambda
# FROM public.ecr.aws/lambda/python:3.11

# # Copy only requirements first (so dependencies are cached if unchanged)
# COPY requirements.txt .

# # Install Python dependencies
# RUN pip install --no-cache-dir -r requirements.txt

# # Now copy your source code (excluding tests via .dockerignore)
# #LAMBDA_TASK_ROOT AWS envirnoment variable
# COPY . ${LAMBDA_TASK_ROOT}

# # Command to run your Lambda handler (e.g. main.handler)
# CMD ["main.handler"]

