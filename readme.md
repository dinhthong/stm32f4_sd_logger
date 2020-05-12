# Todos:
- Periodically check if SD card is present, if not don't do the logging.
- Ardupilot: ARM -> start logging, should implement similarly base on the idea that system should only log when commanded, otherwise we could mistakenly lose the log files.
- Code for a more consistent logging system: how to add a variable to log system, logged data UID, sample rate,....
- Implement benchmark testing to the system.
- Write variables name at the first row
- Write system information to log file (firmware version, frame), date. Or we could utilize to log the messages from the vehicle.
