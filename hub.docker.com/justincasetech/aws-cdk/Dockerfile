FROM justincasetech/gradle:6.0.1

RUN set -e;\
 apk add --no-cache nodejs=10.16.3-r0;\
 apk add --no-cache --virtual deps npm=10.16.3-r0;\
 npm c set unsafe-perm true;\
 npm i -g aws-cdk@1.18.0;\
 apk del deps
