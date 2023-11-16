# Golang Rocket Chat REST API client

Use this simple client if you need to connect to Rocket Chat 
in Golang.

## How to use

Just import

```go
import (
	"github.com/wfunc/gorocket"
)
```

Create client
```go
client := gorocket.NewClient("https://your-rocket-chat.com")

// login as the main admin user
login := gorocket.LoginPayload{
    User:     "admin-login",
    Password: "admin-password",
}

lg, err := client.Login(&login)

if err != nil {
    fmt.Printf("Error: %+v", err)
}
fmt.Printf("I'm %s", lg.Data.Me.Username)
```

or 

```go
client := gorocket.NewWithOptions("https://your-rocket-chat.com", 
    gorocket.WithUserID("my-user-id"),
    gorocket.WithToken("my-bot-token"),
    gorocket.WithTimeout(1 * time.Second),
)
```

## Manage user
```go
str := gorocket.NewUser{
    Email:                 "test@email.com",
    Name:                  "John Doe",
    Password:              "simplepassword",
    Username:              "johndoe",
    Active:                true,
}

me, err := client.UsersCreate(&str)
if err != nil {
    fmt.Printf("Error: %+v", err)
}
fmt.Printf("User was created %t", me.Success)
```

## Post a message
```go
// create a new channel
str := gorocket.CreateChannelRequest{
    Name:     "newchannel",
}

channel, err := client.CreateChannel(&str)
if err != nil {
    fmt.Printf("Error: %+v", err)
}
fmt.Printf("Channel was created %t", channel.Success)
// post a message
str := gorocket.Message{
    Channel:     "somechannel",
    Text:        "Hey! This is new message from Golang REST Client",
}

msg, err := client.PostMessage(&str)
if err != nil {
    fmt.Printf("Error: %+v", err)
}
fmt.Printf("Message was posted %t", msg.Success)
```

## Pagination
If endpoint support pagination, you can use that like this:
```go
// sort field in map. 1 - asc, -1 - desc
srt := map[string]int{"_updatedAt": 1, "name": -1}

client.Count(10).Offset(10).Sort(srt).ChannelList()
```
## PS
Feel free to create issue for add new endpoint to this client