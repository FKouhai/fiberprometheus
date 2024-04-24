# fiberprometheus

Prometheus middleware for gofiber.

Forked from [ansrivas](https://github.com/ansrivas/fiberprometheus) <br>

Please feel free to open any issue with possible enhancements or bugs

**Note: Requires Go 1.22 and above**

![Release](https://img.shields.io/github/release/FKouhai/fiberprometheus.svg)
![Test](https://github.com/FKouhai/fiberprometheus/workflows/Test/badge.svg)
![Security](https://github.com/FKouhai/fiberprometheus/workflows/Security/badge.svg)
![Linter](https://github.com/FKouhai/fiberprometheus/workflows/Linter/badge.svg)

Following metrics are available by default:

```
http_requests_total
http_request_duration_seconds
http_requests_in_progress_total
http_cache_results
```

### Install v3

```
go get -u github.com/gofiber/fiber/v3
go get -u github.com/FKouhai/fiberprometheus/v3
```

### Example using v3

```go
package main

import (
	"github.com/FKouhai/fiberprometheus/v3"
	"github.com/gofiber/fiber/v3"
)

func main() {
  app := fiber.New()

  // This here will appear as a label, one can also use
  // fiberprometheus.NewWith(servicename, namespace, subsystem )
  // or
  // labels := map[string]string{"custom_label1":"custom_value1", "custom_label2":"custom_value2"}
  // fiberprometheus.NewWithLabels(labels, namespace, subsystem )
  prom := prometheus.New("my-service-name")
  prom.RegisterAt(&app, "/metrics")
  app.Use(prom.Middleware)

  app.Get("/", func(c *fiber.Ctx) error {
    return c.SendString("Hello World")
  })

  app.Post("/some", func(c *fiber.Ctx) error {
    return c.SendString("Welcome!")
  })

  app.Listen(":3000")
}
```

### Result

- Hit the default url at http://localhost:3000
- Navigate to http://localhost:3000/metrics

### Grafana Board

- https://grafana.com/grafana/dashboards/14331
