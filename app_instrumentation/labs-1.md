# Lab - Application Instrumentation 

## Task1 
We are build an `API` for an `ecommerce` application in `Python` using the `Flask` library. Take a look at the `/root/main.py` fiel to see the base configuration. There are a total of `4` endpoints. 

- main.py 

```
from prometheus_client
from flask import Flask

"""
Question 3: PLACEHOLDER
"""

"""
Question 10: PLACEHOLDER-1
"""

"""
Question 13: PLACEHOLDER-1
"""

def before_request():
    """
    Question 13: PLACEHOLDER-2
    """

def after_request(response):
    """
    Question 13: PLACEHOLDER-3
    """
    return response

app = Flask(__name__)


@app.get("/products")
def get_products():
    REQUESTS.inc()
    return "product"


@app.post("/products")
def create_product():
    REQUESTS.inc()
    return "created product", 201


@app.get("/cart")
def get_cart():
    REQUESTS.inc()
    return "cart"


@app.post("/cart")
def create_cart():
    REQUESTS.inc()
    return "created cart", 201


@app.errorhandler(404)
def page_not_found(e):
    """
    Question 10: PLACEHOLDER-2
    """
    return "page not found", 404


if __name__ == '__main__':
    """
    Question 5: PLACEHOLDER
    """
    app.run(debug=False, host="0.0.0.0", port='6000')
```

## Task 2 
To instrument this `application`, we will make use of the `prometheus-client` python library. Install the same by running the following command on `prometheus-server`: 

- install prometheus client-library provided for python 

```shell 
pip install prometheus-client 

root@prometheus-server ~ via 🐍 v3.8.10 ✖ pip install prometheus-client 
Collecting prometheus-client
  Downloading prometheus_client-0.21.1-py3-none-any.whl (54 kB)
     |████████████████████████████████| 54 kB 3.5 MB/s 
Installing collected packages: prometheus-client
Successfully installed prometheus-client-0.21.1
```

## Task 3 
Update the `/root/main.py` file to replace the `Question 3: PLACEHOLDER` section with a `Counter` metric called `http_requests_total`. Find more details below: 

(a) First, import the `Counter` object from the `prometheus-client` library.

```python 
from prometheus_client import Counter 
```

(b) Create a `Counter` object and name it as `REQUESTS`.
(i) Metric name should be : `http_requests_total`.
(ii) Description should be `Total number of requests`.
```python 
REQUESTS = Counter('http_requests_total', "Total number of requests')
```

### ANS:
```python 
from flask import Flask
from prometheus_client import Counter

REQUESTS = Counter('http_requests_total', 'Total number of requests')

// ...
```

## Task 5 
Update the `/root/main.py` file to replace the `Question 5: PLACEHOLDER` section with the required code block to expose the metrics on an endpoint using the `start_http_server` object from the `prometheus_client` library. Find more details below: 

(a) Impor the `start_http_server` object from the `prometheus_client` library.

```python 
from prometheus_client import Counter, start_http_server
```


(b) Start the `http` server and make sure it listens on port `8000`. 

```python 
# this gonna expose current python app process an extra port
# python prometheus_client will push the collected metrics to the correspoinding port
start_http_server(8000)
```

## Task 7 
Currently, the `http_request_total` metric does not provide the capability to track requests per `route/endpoint`. Add functionality to trace the number of requests to each route and each `http` method using labels. Please find more details below: 
(a) Add two labels `path` and `method` to the `REQUESTS` counter. It should look like as below: 
```shell 
REQUESTS = Counter('http_requests_total', 'Total number of requests', labelnames = ['path', 'method'])
```

(b) Add `.labels()` to each `REQUETS.inc()` to pass in path and method. For example, `REQUESTS.labels('products', 'get').inc()` where `products` are a `path` and `get` is a `method`. 

(c) Run the `flask.sh` command to restart the app. 

## Task 8 
Configure a new job named `api` to scrape our Flask application and restart the `Prometheus`. 

- `vim /etc/prometheus/prometheus.yml`

```yml 
scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  - job_name: "api"
    static_configs:
      - targets: ["localhost:8000"]
  - job_name: "prometheus"

    # metrics_path defaults to '/metrics'
    # scheme defaults to 'http'.

```

- sudo systemctl restart prometheus 

```shell 
root@prometheus-server ~ via 🐍 v3.8.10 ➜  systemctl restart prometheus 

root@prometheus-server ~ via 🐍 v3.8.10 ➜  systemctl status prometheus 
● prometheus.service - Prometheus
     Loaded: loaded (/etc/systemd/system/prometheus.service; enabled; vendor preset: enabled)
     Active: active (running) since Sat 2025-04-26 06:16:11 GMT; 6s ago
       Docs: https://prometheus.io/docs/introduction/overview/
   Main PID: 6302 (prometheus)
      Tasks: 21 (limit: 77143)
     Memory: 31.9M
     CGroup: /system.slice/prometheus.service
             └─6302 /usr/local/bin/prometheus --config.file=/etc/prometheus/prometheus.yml --storage.tsdb.path>

Apr 26 06:16:11 prometheus-server prometheus[6302]: ts=2025-04-26T06:16:11.854Z caller=head.go:683 level=info >
Apr 26 06:16:12 prometheus-server prometheus[6302]: ts=2025-04-26T06:16:12.048Z caller=head.go:683 level=info >
Apr 26 06:16:12 prometheus-server prometheus[6302]: ts=2025-04-26T06:16:12.048Z caller=head.go:683 level=info >
Apr 26 06:16:12 prometheus-server prometheus[6302]: ts=2025-04-26T06:16:12.048Z caller=head.go:720 level=info >
Apr 26 06:16:12 prometheus-server prometheus[6302]: ts=2025-04-26T06:16:12.051Z caller=main.go:1014 level=info>
Apr 26 06:16:12 prometheus-server prometheus[6302]: ts=2025-04-26T06:16:12.051Z caller=main.go:1017 level=info>
Apr 26 06:16:12 prometheus-server prometheus[6302]: ts=2025-04-26T06:16:12.051Z caller=main.go:1197 level=info>
Apr 26 06:16:12 prometheus-server prometheus[6302]: ts=2025-04-26T06:16:12.051Z caller=main.go:1234 level=info>
Apr 26 06:16:12 prometheus-server prometheus[6302]: ts=2025-04-26T06:16:12.052Z caller=main.go:978 level=info >
Apr 26 06:16:12 prometheus-server prometheus[6302]: ts=2025-04-26T06:16:12.052Z caller=manager.go:944 level=in>
lines 1-20/20 (END)
```

--- 

- Final main.py 
```python 
from flask import Flask
from prometheus_client import Counter, start_http_server, Gauge 

REQUESTS = Counter('http_requests_total', 'Total number of requests', labelnames=['path', 'method'])
IN_PROGRESS = Gauge('inprogress_requests', 'Total number of requests in progress')


"""
Question 10: PLACEHOLDER-1
"""

# here we create counter for error metrics 
ERRORS = Counter('http_error_total', 'Total number of errors', labelnames=['code'])

"""
Question 13: PLACEHOLDER-1
"""

def before_request():
    IN_PROGRESS.inc()
    """
    Question 13: PLACEHOLDER-2
    """

def after_request(response):
    """
    Question 13: PLACEHOLDER-3
    """
    IN_PROGRESS.dec()
    return response

app = Flask(__name__)


@app.get("/products")
def get_products():
    """
    Question 4: PLACEHOLDER-1
    """
    REQUESTS.labels('products', 'get').inc()
    return "product"


@app.post("/products")
def create_product():
    """
    Question 4: PLACEHOLDER-2
    """
    REQUESTS.labels('products', 'post').inc()
    return "created product", 201


@app.get("/cart")
def get_cart():
    """
    Question 4: PLACEHOLDER-3
    """
    REQUESTS.labels('cart', 'get').inc()
    return "cart"


@app.post("/cart")
def create_cart():
    """
    Question 4: PLACEHOLDER-4
    """
    REQUESTS.labels('cart', 'post').inc()
    return "created cart", 201


@app.errorhandler(404)
def page_not_found(e):
    """
    Question 10: PLACEHOLDER-2
    """
    ERRORS.labels('404').inc()
    return "page not found", 404


if __name__ == '__main__':
    """
    Question 5: PLACEHOLDER
    """
    app.run(debug=False, host="0.0.0.0", port='6000')
    start_http_server(8000)
```



















