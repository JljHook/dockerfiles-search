FROM marmotcai/golang AS golang-builder

MAINTAINER marmotcai "marmotcai@163.com"

ENV APP_GIT_URL="github.com/minio/minio"
RUN gopm get -g -v $APP_GIT_URL
#RUN gopm bin
#RUN gopm clean
#COPY golang/build.sh $GOBIN/build
#RUN wget -O $GOBIN/build https://raw.githubusercontent.com/marmotcai/gather/master/golang/build.sh
#RUN chmod +x $GOBIN/build
VOLUME ["/root/output"]
WORKDIR /root

#CMD ["build"]

