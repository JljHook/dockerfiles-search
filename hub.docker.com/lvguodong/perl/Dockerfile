FROM perl:latest
MAINTAINER 1071858797@qq.com
RUN cpan App::cpanminus && sed -i "s/http:\/\/www.cpan.org/https:\/\/mirrors.aliyun.com\/CPAN/g" ~/.cpan/CPAN/MyConfig.pm && cat ~/.cpan/CPAN/MyConfig.pm && cpanm Test::More
