FROM interstore/data-space-connector-ui:1.0.6

ARG API_URL
ENV API_URL=${API_URL}

# When the container starts, replace the env.js with values from environment variables
RUN envsubst < /usr/share/nginx/html/env.template.js > /usr/share/nginx/html/env.js