FROM alpine:3.5
RUN \
  apk add --update openssl expat-dev openldap-dev libxml2-dev perl perl-dev apkbuild-cpan heimdal-telnet openssh-client gcc make g++ && \
  yes | cpan -i App::cpanminus && \
  cpanm Net::Ping Crypt::Blowfish inc::latest Data::Serializer Env Fatal IO::Socket::IP IO::Socket::SSL \
    Config::General Net::Telnet AnyEvent::RabbitMQ Mojo::JSON Mojo::UserAgent Crypt::Cipher::AES XML::Simple \
    IO::Pty Digest::SHA Net::LDAPS Net::LDAP::Util File::HomeDir Crypt::CBC Crypt::OpenSSL::AES \
    Module::ScanDeps PAR App::Packer::PAR && \
  apk del gcc make g++ perl-dev && \
  rm -Rf /var/cache/apk/* /root/.cpan /tmp/*
