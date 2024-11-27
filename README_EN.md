# Go/Gin Gower Workspace Directory

![](.github/images/logo.png)\`workspace\`

[Chinese](README.md)|[English](README_EN.md)

[![license](https://img.shields.io/badge/license-MIT-green?style=flat-square&logo=MIT)](LICENSE) [![benchmark](https://img.shields.io/badge/gower-benchmark-red?style=flat-square&logo=Sencha)]() [![actions](https://img.shields.io/badge/github-actions-green?style=flat-square&logo=GitHub)](https://github.com/falling-ts/gower/actions) [![version](https://img.shields.io/badge/version-0.8.2-yellow?style=flat-square&logo=V)]()

---

The Go/Gin Gower Workspace is a workspace mode implementation based on the Go language and GOPATH. It uses Gradle to manage the development and packaging of multiple Gower programs, achieving a microservices-like architecture.

System Requirements:

> go >= v1.23
>
> nodejs >= v16.13
>
> pnpm >= v7.0
>
> docker >= v20.10 [optional]
>
> docker compose >= v2.0 [optional]
>
> git >= 2.39
>
> gradle == 8.10.2
>
> jdk >= 23

## Quick Start [Workspace Mode]

### Download Source Code

```shell
$ git clone -b v0.8.2 --single-branch --depth 1 https://github.com/falling-ts/gower-work.git
```


### Open `gower-work` with GoLand

- Install the Gradle plugin in GoLand beforehand.
- When opening `gower-work` for the first time, you will be prompted to `Found Gradle 'gower-work' build script`, then click `Load Gradle project` to initialize the Gradle build system.
- Find `Settings/GO/GOPATH/Project GOPATH`, and add the absolute path of the `gower-work` directory.
- This way, when executing `go install` within the project, it will automatically install the binary to the `bin` directory.
- The `bin` directory under the project will also be added to the environment variables of the built-in terminal, making it convenient to execute commands.

### Install Gower CLI Tool

- Find the Gradle toolbar on the right.
- Locate `gower-work/Tasks/init/InstallGowerCli`, double-click to run, which will install `gower.exe` to the `bin` directory.

### Create a New Gower Project

- In the terminal of the `gower-work` project root directory, execute:
```shell
$ gower create my-project
```

- The output will be as follows:
```shell
src\my-project project created successfully
---------------- Initializing APP key...:
APP key generated successfully
---------------- Initializing JWT key...:
JWT key generated successfully
---------------- Downloading Go dependencies...:
---------------- Downloading frontend dependencies...:
Lockfile is up to date, resolution step is skipped
......
+ @types/vue 2.0.0
+ @vitejs/plugin-vue 4.0.0
+ cross-env 7.0.3
+ vite 5.4.6

Done in 1.2s
---------------- Initializing Git repository...: 
Initialized empty Git repository in D:/Go/gower-work/src/my-project/.git/
---------------- Adding all files...: 
---------------- Initial commit...: 
......
create mode 100644 utils/slice/strings.go
create mode 100644 utils/str/str.go
create mode 100644 vite.config.js
---------------- Building frontend library files...:
......
public/static/main.umd.cjs    640.00 kB  gzip: 211.13 kB
 built in 4.88s
npm notice
npm notice New minor version of npm available! 10.7.0 -> 10.9.0
npm notice Changelog: https://github.com/npm/cli/releases/tag/v10.9.0
npm notice To update run: npm install -g npm@10.9.0
npm notice
---------------- Adding working directory...:
---------------- Running benchmark tests...: 
2023-10-17 12:48:37, ERROR, Gower, Error Trace, {"error": "record not found", "sql": "SELECT * FROM `admin
_users` WHERE `admin_users`.`id` = 1 AND `admin_users`.`deleted_at` IS NULL ORDER BY `admin_users`.`id` LIMIT 1", "elapsed": 0, "rows": 0}
......
Benchmark/BenchmarkEncode-12             1896417               628.9 ns/op
Benchmark/BenchmarkDecode-12             3177018               389.5 ns/op
PASS
ok      my-project      25.313s
```


### Run the Project

- In the Gradle tasks, find `my-project/Tasks/dev/Run`, and double-click to run.
- The output will be as follows:
```shell
......
> Task :my-project:t0300GoBuild
# go build -tags tmpl,static -o my-project.exe

> Task :my-project:Run
# my-project.exe run
http://localhost:8080

BUILD SUCCESSFUL in 10s
6 actionable tasks: 6 executed
00:57:32: Completed 'Run'.
```

- Click `http://localhost:8080` to open the browser and access the project.

## Quick Development

### Create a Controller

```shell
$ gower make --controller Hello
```


`app/http/controllers/hello_controller.go`

```shell
package controllers

import (
    "gower/app"
    "gower/app/http/requests"
    "gower/services"
)

type HelloController struct {
    app.Controller
}

var Hello = new(HelloController)

// Index Get page
func (*HelloController) Index(req *requests.HelloRequest) (services.Response, error) {
    return res.Ok("home/hello", app.Data{
        "name": req.Name,
    }), nil
}
```


### Create a Request

```shell
$ gower make --request Hello
```


`app\http\requests\hello_request.go`

```shell
package requests

import "gower/app"

type HelloRequest struct {
    app.Request

    Name *string `form:"name" json:"name" binding:"required"`
}
```


### Create a Model

```shell
$ gower make --model Hello
```


`app\models\hello.go`

```shell
package models

func init() {
    migrate(new(Hello))
}

type Hello struct {
    Model

    Name *string `gorm:"type:string;default:'';comment:Name"`
}
```


> Note: If the command outputs a lot of debug information, it's because the `APP_MODE` in `envs/.env.dev` is set to development mode. Change it to test mode.

### Add Routes

`routes/web.go`

```shell
package routes

import (
    web "gower/app/http/controllers"
    mws "gower/app/http/middlewares"
    "gower/public"
)

func init() {
    // ...

    route.GET("/hello", web.Hello.Index)
}
```


### Execute the Request

```shell
$ curl -i http://localhost:8080/hello?name=Gower
```


## Third-Party Libraries and Content, Expressing Thanks to Open Source

```
github.com/alexedwards/argon2id v1.0.0
github.com/caarlos0/env/v7 v7.1.0
github.com/gin-contrib/cors v1.7.2
github.com/gin-gonic/gin v1.10.0
github.com/glebarez/sqlite v1.11.0
github.com/go-playground/locales v0.14.1
github.com/go-playground/universal-translator v0.18.1
github.com/go-playground/validator/v10 v10.22.1
github.com/go-sql-driver/mysql v1.8.1
github.com/golang-jwt/jwt/v5 v5.2.1
github.com/jaevor/go-nanoid v1.4.0
github.com/joho/godotenv v1.5.1
github.com/patrickmn/go-cache v2.1.0+incompatible
github.com/stretchr/testify v1.9.0
github.com/urfave/cli/v2 v2.27.5
go.uber.org/zap v1.27.0
golang.org/x/crypto v0.28.0
gorm.io/driver/mysql v1.5.7
gorm.io/gorm v1.25.12

github.com/rclone/rclone v1.62.2
github.com/laravel/laravel
github.com/moby/moby
github.com/docker/compose

FROM caddy:2.6
FROM grafana/grafana:9.4.3
FROM grafana/loki:main-0295fd4
FROM mysql/mysql-server:5.7.41
FROM grafana/promtail:main-0295fd4
FROM pingcap/tidb:v6.5.1

nodejs
pnpm
"animate.css": "^4.1.1",
"autoprefixer": "^10.4.13",
"daisyui": "^2.51.2",
"jquery": "^3.6.3",
"js-cookie": "^3.0.1",
"jssha": "^3.3.0",
"postcss": "8.4.31",
"resize-observer-polyfill": "^1.5.1",
"simplebar": "^6.2.1",
"stylus": "^0.59.0",
"tailwindcss": "^3.2.7",
"vue": "^3.2.47"
"@rollup/plugin-replace": "^5.0.2",
"@types/crypto-js": "^4.1.1",
"@types/jquery": "^3.5.16",
"@types/js-cookie": "^3.0.3",
"@types/node": "^18.15.10",
"@types/vue": "^2.0.0",
"@vitejs/plugin-vue": "^4.0.0",
"cross-env": "^7.0.3",
"vite": "5.4.6"
```


## Documentation

[Documentation Address](https://falling-ts.github.io/gower-docs)

## LICENSE

[MIT License](LICENSE)

## Home Page

![](.github/images/home.png)

## Sample Themes

By modifying `VIEW_THEME` in `.env.xxx`, see [DaisyUI](https://daisyui.com/docs/themes/) for details.

### cupcake

![](.github/images/cupcake.png)

### forest

![](.github/images/forest.png)

### halloween

![](.github/images/halloween.png)

### lofi

![](.github/images/lofi.png)

### synthwave

![](.github/images/synthwave.png)