

# Base image for Python Lambda
FROM public.ecr.aws/lambda/python:3.11

# Install build dependencies (for PyMuPDF)
RUN yum install -y gcc g++ make && yum clean all

# Copy only requirements first (so dependencies are cached if unchanged)
COPY requirements.txt .

# Install Python dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Now copy your source code (excluding tests via .dockerignore)
#LAMBDA_TASK_ROOT AWS envirnoment variable
COPY . ${LAMBDA_TASK_ROOT}

# Command to run your Lambda handler (e.g. main.handler)
CMD ["main.handler"]
