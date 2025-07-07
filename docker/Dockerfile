FROM python:3.12-slim

WORKDIR /app

# Install system dependencies
RUN apt-get update && apt-get install -y \
    gcc \
    && rm -rf /var/lib/apt/lists/*

# Copy requirements and install Python packages
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy app
COPY dataset_agent_streamlit_app.py .

# Create /app/data and set permissions for non-root user
RUN mkdir -p /app/data && useradd -m -u 1000 streamlit && chown -R streamlit:streamlit /app/data
USER streamlit

# Expose port
EXPOSE 8501

# Health check
HEALTHCHECK CMD curl --fail http://localhost:8501/_stcore/health

# Run the app
ENTRYPOINT ["streamlit", "run", "dataset_agent_streamlit_app.py", "--server.port=8501", "--server.address=0.0.0.0"]
