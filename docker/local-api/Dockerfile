FROM interstore/local-api:1.2.0

EXPOSE 30001

ARG CENTRAL_REGISTRY
ENV CENTRAL_REGISTRY_ENV=${CENTRAL_REGISTRY}

ARG DATA_PART_SIZE
ENV DATA_PART_SIZE_ENV=${DATA_PART_SIZE}

ARG DATA_SUBPART_NUMBER
ENV DATA_SUBPART_NUMBER_ENV=${DATA_SUBPART_NUMBER}

RUN touch ./entrypoint.sh
RUN echo "java -jar ./app.jar --sofia.uri=${CENTRAL_REGISTRY_ENV} --data.part.size=${DATA_PART_SIZE_ENV} --data.subPart.number=${DATA_SUBPART_NUMBER_ENV}" > ./entrypoint.sh
RUN chmod +x ./entrypoint.sh
CMD ./entrypoint.sh
