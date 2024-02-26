<div style="text-align: center">

# go-unifi-protect

> go-unifi-protect is a Golang client library for the UniFi Protect API, designed to offer developers a convenient way
> to interact with UniFi Protect devices and services programmatically. This library covers critical functionality,
> including authentication, management of sensors and users, live updates, and event handling.

</div>

---

## Table of Contents

<!-- TOC -->

* [go-unifi-protect](#go-unifi-protect)
  * [Table of Contents](#table-of-contents)
  * [Features](#features)
  * [Project Structure](#project-structure)
  * [Getting Started](#getting-started)
    * [Prerequisites](#prerequisites)
    * [Installation](#installation)
    * [Usage](#usage)
      * [Creating a Client](#creating-a-client)
      * [Authentication](#authentication)
      * [Working with Sensors](#working-with-sensors)
      * [Handling User Information](#handling-user-information)
      * [Subscribing to Live Updates](#subscribing-to-live-updates)
  * [API Resources](#api-resources)
  * [Contributing](#contributing)
  * [License](#license)

<!-- TOC -->
---

## Features

* **Authentication**: Authenticate securely against the UniFi Protect API.
* **User Management**: Retrieve and manage user accounts.
* **Sensor Interaction**: List and control sensor devices.
* **Live Updates**: WebSockets integration for real-time event monitoring.
* **Event Decoding**: Decode WebSocket messages for event handling.

## Project Structure

* `api/v1/`: Contains the API definitions and interactions.
* `assets/`: Holds project assets such as images and logos.

## Getting Started

### Prerequisites

* Go environment
* UniFi Protect system access

### Installation

```bash
go get github.com/yourgithubusername/go-unifi-protect
```

### Usage

To use `go-unifi-protect` in your project, you must import the library and use its structures and methods.

#### Creating a Client

Before you can interact with the UniFi Protect API, you need to create a new client. The following code demonstrates how
to instantiate a client:

```go
import (
    "net/url"
    "github.com/yourgithubusername/go-unifi-protect"
)

func main() {
    baseURL, _ := url.Parse("https://your-unifi-protect-url.com")
    auth := go_unifi_protect.NewAuth("your_username", "your_password")
    client := go_unifi_protect.NewClient(baseURL, auth)
}
```

#### Authentication

Use the `Auth` structure to authenticate with the UniFi Protect API by providing your credentials:

```go
ctx := context.Background()
userAuthError := client.Authenticate(ctx)

if userAuthError != nil {
    log.Fatalf("Authentication error: %s", userAuthError)
}
```

#### Working with Sensors

To interact with sensors, the `ApiClient` provides methods to list sensors and retrieve individual sensor details:

```go
sensors, sensorsError := client.V1().Sensors.List(ctx)

if sensorsError != nil {
    log.Fatalf("Error retrieving sensors: %s", sensorsError)
}

for _, sensor := range sensors {
    fmt.Printf("Sensor: %v\n", sensor)
}
```

#### Handling User Information

Retrieve details about the current user:

```go
currentUser, userError := client.V1().Users.Self(ctx)

if userError != nil {
    log.Fatalf("Error retrieving user information: %s", userError)
}

fmt.Printf("User: %v\n", currentUser)
```

#### Subscribing to Live Updates

The library also supports connecting to WebSocket endpoints for real-time updates:

```go
updatesChan := make(chan *go_unifi_protect.WsFrame)

go func () {
    for update := range updatesChan {
        fmt.Printf("Received update: %v\n", update)
    }
}()

// Initiating live updates subscription
client.V1().Live.Updates(ctx, &go_unifi_protect.UpdateRequest{LastUpdateId: "your_update_id"}, updatesChan)
```

For more complex usage examples, you can refer to the [examples directory](./examples) in the repository.

## API Resources

* `Auth`: Authentication and session management
* `Users`: User account interactions
* `Sensors`: Sensor list and management
* `Live`: Real-time updates via WebSockets
* `Events`: Event-related APIs

## Contributing

Feel free to fork the repository, make changes, and submit pull requests. Your contributions make the open-source
community thrive.

## License

This project is released under the MIT License - see the [`LICENSE`](./LICENSE) file for details.
