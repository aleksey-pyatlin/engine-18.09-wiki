Telemetry phones home at engine start, then every 24 hours. Also included with 18.03.1-ee-3. The data reported is identical to that with Linux EE engines.

* Opt-out

Set `telemetry` to false in `features` within the engine configuration:

`echo '{"features":{"telemetry":false}}' > %PROGRAMFILES%\docker\daemon.json`

* To verify telemetry state, at startup look for the following line in the daemon log:

`INFO[2018-08-23T14:58:21.443100742Z] starting telemetry service`
