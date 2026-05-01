FROM python:3.13-slim

# Combined into a single ENV instruction to reduce layers
ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    MICRO_SERVICE=/home/app/webapp

# Create a non-root user and group for security
RUN addgroup --system app && adduser --system --group app

# Set work directory (WORKDIR automatically creates the directory if it doesn't exist)
WORKDIR $MICRO_SERVICE

# OPTIMIZATION: Copy ONLY requirements.txt first to leverage Docker layer caching
COPY --chown=app:app src/requirements.txt .

# Install dependencies
# Added --no-cache-dir to prevent pip from storing downloaded packages, reducing image size
RUN pip install --upgrade pip --no-cache-dir && \
    pip install -r requirements.txt --no-cache-dir

# Copy the rest of the application code
COPY --chown=app:app src/ .

# Switch to the non-root user
USER app

# Expose the standard Streamlit port
EXPOSE 8501

# Use the exec form for CMD and bind Streamlit to 0.0.0.0
CMD ["streamlit", "run", "app.py", "--server.port=8501", "--server.address=0.0.0.0"]