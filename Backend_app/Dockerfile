

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



# Use an official AWS Lambda Python base image.
# Replace `3.x` with your target Python version (e.g., `3.11`).
FROM public.ecr.aws/lambda/python:3.11

# Copy the requirements.txt file and install dependencies.
# The `pymupdf` library requires some system-level dependencies.
# We use a multi-stage build to keep the final image size smaller.
# The `build-image` stage is optimized for installing dependencies.
FROM public.ecr.aws/lambda/python:3.11 AS build-image

# Install system dependencies needed by some Python libraries (e.g., pymupdf).
RUN yum install -y poppler-cpp-devel

# Set the working directory inside the container.
WORKDIR /var/task

# Copy the requirements file into the build stage.
COPY requirements.txt .

# Install Python packages from requirements.txt into the /var/task directory.
# `pip install --no-cache-dir` is recommended for production images.
RUN pip install -r requirements.txt --target /var/task

# Use the base image for the final, lean image.
FROM public.ecr.aws/lambda/python:3.11

# Copy the installed dependencies from the build-image stage.
COPY --from=build-image /var/task /var/task

# Copy your Lambda function code into the container.
# Make sure your Python file (e.g., `app.py`) is in the same directory.
COPY . ${LAMBDA_TASK_ROOT}

# Set the CMD to your handler.
# The format is "[<file_name>.<function_name>]".
# For a file `app.py` with a function `handler`, the CMD is ["app.handler"].
CMD ["main.handler"]


