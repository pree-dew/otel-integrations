### Sending data from s3 to otel collector via Logstash

**Problem Statement**
- There are logs stored on s3 in json format and gz compression, want to ship these logs to any otel compatible backend
  
**Flow**

   ![image](https://github.com/user-attachments/assets/8262adf6-6b2c-423a-993e-a78a2ce3a302)

**Instructions to Run setup**

- Edit docker-compose with relevant environment variable.
- Run `cp logstash.conf /tmp/.`
- Run `docker-compose up > /tmp/logs`

**Verify that logs are shipping**
- By logs: We can check `/tmp/logs` file as per last command to see the logs as otel collector config has debug as exporter so all logs will be visible in `/tmp/logs`
- By logstash:
  - `docker exec -it logstash bash`
  -  `cd /usr/share/logstash/data/plugins/inputs/s3/`
  -  Above directory will have a file starting with `since_db`, run `cat sincedb_xxxxx`, this will show the timestamp of last parsed file
  -  To see what all files are processed `cd /tmp/backup && ls`
 
**Pros of approach**
- Most of the features that we will need for the problem are already handled by Logstash like retries, state maintenance, frequency, backup of processed files etc
- It works with s3 using `role_arn` also which is a requirement by many customer.

**Cons of approach**
- With scale only vertical scaling is possible as the `since_db` file is maintened locally on node/pod.
- If any files fails in between then it will end up creating duplicating data on re-run, as the state is updated after full file is processed.
- Logstash doesn’t support conversion of logs in otlp format in http plugin and collector only accepts data in otlp format, so custom code / processing is required in logstash configuration.

**Limitations**
- Handle only .gz file.
- For `role_arn` only env or config file is the way to give default user access, it doesn’t work in logstash conf.
  
