FROM maven:3-jdk-8-slim

# skip installing gem documentation

COPY m2 $HOME
RUN mv m2 .m2
RUN cd $HOME && tar xvzf .m2.tar.gz
RUN cd $HOME .m2.tar.gz

EXPOSE 8080
