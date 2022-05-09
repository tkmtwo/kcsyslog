FROM cnfldemos/cp-server-connect-datagen:0.5.3-7.1.0

RUN confluent-hub install --no-prompt --component-dir /usr/share/confluent-hub-components confluentinc/kafka-connect-syslog:latest
RUN confluent-hub install --no-prompt --component-dir /usr/share/confluent-hub-components wepay/kafka-connect-bigquery:2.3.0


