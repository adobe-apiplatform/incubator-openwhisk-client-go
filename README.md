<!--
#
# Licensed to the Apache Software Foundation (ASF) under one or more
# contributor license agreements.  See the NOTICE file distributed with
# this work for additional information regarding copyright ownership.
# The ASF licenses this file to You under the Apache License, Version 2.0
# (the "License"); you may not use this file except in compliance with
# the License.  You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#
-->

# Openwhisk Client Go
[![License](https://img.shields.io/badge/license-Apache--2.0-blue.svg)](http://www.apache.org/licenses/LICENSE-2.0)
[![Build Status](https://travis-ci.org/apache/incubator-openwhisk-client-go.svg?branch=master)](https://travis-ci.org/apache/incubator-openwhisk-client-go)

This project `openwhisk-client-go` is a Go client library to access Openwhisk API.


### Usage

```go
import "github.com/apache/incubator-openwhisk-client-go/whisk"
```

Construct a new whisk client, then use various services to access different parts of the whisk api.  For example to get the `hello` action:

```go
client, _ := whisk.NewClient(http.DefaultClient, nil)
action, resp, err := client.Actions.List("hello")
```

Some API methods have optional parameters that can be passed. For example, to list the first 30 actions, after the 30th action:
```go
client, _ := whisk.NewClient(http.DefaultClient, nil)

options := &whisk.ActionListOptions{
  Limit: 30,
  Skip: 30,
}

actions, resp, err := client.Actions.List(options)
```

Whisk can be configured by passing in a `*whisk.Config` object as the second argument to `whisk.New( ... )`.  For example:

```go
u, _ := url.Parse("https://whisk.stage1.ng.bluemix.net:443/api/v1/")
config := &whisk.Config{
  Namespace: "_",
  AuthKey: "aaaaa-bbbbb-ccccc-ddddd-eeeee",
  BaseURL: u
}
client, err := whisk.Newclient(http.DefaultClient, config)
```


### Example
```go
import (
  "net/http"
  "net/url"

  "github.com/apache/incubator-openwhisk-client-go/whisk"
)

func main() {
  client, err := whisk.NewClient(http.DefaultClient, nil)
  if err != nil {
    fmt.Println(err)
    os.Exit(-1)
  }

  options := &whisk.ActionListOptions{
    Limit: 30,
    Skip: 30,
  }

  actions, resp, err := client.Actions.List(options)
  if err != nil {
    fmt.Println(err)
    os.Exit(-1)
  }

  fmt.Println("Returned with status: ", resp.Status)
  fmt.Println("Returned actions: \n%+v", actions)

}
```

# Disclaimer
Apache OpenWhisk Client Go is an effort undergoing incubation at The Apache Software Foundation (ASF), sponsored by the Apache Incubator. Incubation is required of all newly accepted projects until a further review indicates that the infrastructure, communications, and decision making process have stabilized in a manner consistent with other successful ASF projects. While incubation status is not necessarily a reflection of the completeness or stability of the code, it does indicate that the project has yet to be fully endorsed by the ASF.
