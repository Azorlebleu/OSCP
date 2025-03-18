```bash
# Send an email
sendEmail -t dave.wizard@supermagicorg.com -f test@supermagicorg.com -s 192.168.223.199 -u "Important Upgrade Instructions" -o message-file=mail-content.txt

# Send an email with attached file
sendEmail -t dave.wizard@supermagicorg.com -f test@supermagicorg.com -s 192.168.223.199 -u "Important Upgrade Instructions" -a config.Library-ms -o message-file=mail-content.txt

# Send an email on the behalf of someone
sendEmail -t dave.wizard@supermagicorg.com -f test@supermagicorg.com -s 192.168.223.199 -u "Important Upgrade Instructions" -o message-file=mail-content.txt username=test@supermagicorg.com password=test

# All together :)
sendEmail -t dave.wizard@supermagicorg.com -f test@supermagicorg.com -s 192.168.223.199 -u "Important Upgrade Instructions" -a config.Library-ms -o message-file=mail-content.txt username=test@supermagicorg.com password=test
```