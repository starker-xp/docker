FROM elasticsearch:latest

RUN echo 'http.cors.enabled: true' >> /usr/share/elasticsearch/config/elasticsearch.yml
RUN echo 'http.cors.allow-origin: "*"' >> /usr/share/elasticsearch/config/elasticsearch.yml
RUN echo 'cluster.name: mycluster' >> /usr/share/elasticsearch/config/elasticsearch.yml