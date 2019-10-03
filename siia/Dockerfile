FROM maven:3-jdk-8-slim

# skip installing gem documentation

RUN echo $HOME
RUN ls -la
COPY m2/repository/* $HOME
RUN cd $HOME && mv m2 .m2

EXPOSE 8080
