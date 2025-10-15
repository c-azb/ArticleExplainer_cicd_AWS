

# Base image for Python Lambda
FROM public.ecr.aws/lambda/python:3.11

# Copy application files
COPY . ${LAMBDA_TASK_ROOT}

# Install dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Command to run your Lambda handler
# Example: if your handler is in app.py -> handler
CMD ["main.handler"]
