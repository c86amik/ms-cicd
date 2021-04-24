FROM openjdk:8-jre-alpine
ENV APP_FILE flight-registration-service-0.6.jar
ENV APP_HOME /usr/apps
EXPOSE 7104
COPY target/$APP_FILE $APP_HOME/
WORKDIR $APP_HOME
ENTRYPOINT ["sh", "-c"]
CMD ["exec java -jar $APP_FILE"]