FROM srcml/centos_x86_64_base:7.3

RUN echo centos_7.3_x86_64 > /name.txt

RUN yum install python-yaml -y

COPY lst_image.py /lst_image.py

WORKDIR /srcML-build

CMD ["/bin/bash", \
     "-c", \
      "cmake -DBUILD_CLIENT_TESTS=OFF -DCMAKE_BUILD_TYPE=Release /srcML; \
      make install; \
      ldconfig; \
      python /lst_image.py"]
