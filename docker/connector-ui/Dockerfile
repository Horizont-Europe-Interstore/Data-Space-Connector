FROM interstore/data-space-connector-ui:latest

ARG API_URL
ENV API_URL=${API_URL}

ARG CENTRAL_API_URL
ENV CENTRAL_API_URL=${CENTRAL_API_URL}

ARG PUSH_ENABLED
ENV PUSH_ENABLED=${PUSH_ENABLED}

ARG DATA_APP_NAME
ENV DATA_APP_NAME=${DATA_APP_NAME}

# When the container starts, replace the env.js with values from environment variables
RUN envsubst < /usr/share/nginx/html/env.template.js > /usr/share/nginx/html/env.js