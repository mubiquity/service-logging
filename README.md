Aggregate structured logs for sending to external service.
Contains implementations for Coralogix and (for wasm) console.log.


## Usage

Use the `log!` macro to log key-value pairs, which are json-encoded
before sending to logging service

```
use service_logging::{log,LogQueue, Severity::{Info,Warning}};
let logger =  CoralogixLogger::init(CoralogixConfig{
    api_key: "0000",
	application_name: "MyApp",
	endpoint: "https://api.coralogix.com/api/v1/logs"});
let mut lq = LogQueue::default();

log!(lq, Info, 
  method: "GET",
  url: url,
  status: 200
);

log!(lq, Error,
  user: user,
  message: "Too many failed login attempts",
  attempts: count
);

send_logs(lq.take(), &logger).expect("send logs");
```
