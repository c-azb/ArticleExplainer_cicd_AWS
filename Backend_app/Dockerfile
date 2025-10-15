

# # Base image for Python Lambda
# FROM public.ecr.aws/lambda/python:3.11

# # Install build dependencies (for PyMuPDF)
# RUN yum install -y gcc g++ make && yum clean all

# # Copy only requirements first (so dependencies are cached if unchanged)
# COPY requirements.txt .

# # Install Python dependencies
# RUN pip install --no-cache-dir -r requirements.txt

# # Now copy your source code (excluding tests via .dockerignore)
# #LAMBDA_TASK_ROOT AWS envirnoment variable
# COPY . ${LAMBDA_TASK_ROOT}

# # Command to run your Lambda handler (e.g. main.handler)
# CMD ["main.handler"]



# ----------- Build stage -----------
FROM public.ecr.aws/lambda/python:3.11 AS build-image

# Install system dependencies needed for building packages
RUN yum install -y gcc gcc-c++ make poppler-cpp-devel && yum clean all

# Set the working directory
WORKDIR /var/task

# Copy requirements and install into /var/task (Lambda’s code directory)
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt --target /var/task


# ----------- Final runtime stage -----------
FROM public.ecr.aws/lambda/python:3.11

# Copy everything from build stage
COPY --from=build-image /var/task /var/task

# Copy your application code
COPY . ${LAMBDA_TASK_ROOT}

# Set the handler (adjust to your filename and function)
CMD ["main.handler"]


