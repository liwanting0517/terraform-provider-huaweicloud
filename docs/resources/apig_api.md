---
subcategory: "API Gateway (Dedicated APIG)"
---

# huaweicloud_apig_api

Manages an APIG API resource within HuaweiCloud.

## Example Usage

```hcl
variable "instance_id" {}
variable "group_id" {}
variable "api_name" {}
variable "custom_response_id" {}
variable "custom_auth_id" {}
variable "vpc_channel_id" {}

resource "huaweicloud_apig_api" "test" {
  instance_id             = var.instance_id
  group_id                = var.group_id
  type                    = "Public"
  name                    = var.api_name
  request_protocol        = "HTTP"
  request_method          = "POST"
  request_path            = "/terraform/users"
  security_authentication = "AUTHORIZER"
  matching                = "Exact"
  success_response        = "Successful"
  response_id             = var.custom_response_id
  authorizer_id           = var.custom_auth_id

  backend_params {
    type     = "SYSTEM"
    name     = "X-User-Auth"
    location = "HEADER"
    value    = "user_name"
  }

  web {
    path             = "/backend/users"
    vpc_channel_id   = var.vpc_channel_id
    request_method   = "POST"
    request_protocol = "HTTP"
    timeout          = 5000
  }
}
```

## Argument Reference

The following arguments are supported:

* `region` - (Optional, String, ForceNew) Specifies the region where the API is located.  
  If omitted, the provider-level region will be used. Changing this will create a new API resource.

* `instance_id` - (Required, String, ForceNew) Specifies an ID of the APIG dedicated instance to which the API belongs
  to. Changing this will create a new API resource.

* `group_id` - (Required, String) Specifies an ID of the APIG group to which the API belongs to.

* `type` - (Required, String) Specifies the API type.  
  The valid values are **Public** and **Private**.

* `name` - (Required, String) Specifies the API name.  
  The valid length is limited from can contain `3` to `64`, only Chinese and English letters, digits and hyphens (-)
  are allowed.  
  The name must start with a Chinese or English letter.

* `request_method` - (Required, String) Specifies the request method of the API.  
  The valid values are **GET**, **POST**, **PUT**, **DELETE**, **HEAD**, **PATCH**, **OPTIONS** and **ANY**.

* `request_path` - (Required, String) Specifies the request address, which can contain a maximum of `512` characters,
  the request parameters enclosed with brackets ({}).  
  + The address can contain special characters, such as asterisks (*), percent signs (%), hyphens (-), and
    underscores (_) and must comply with URI specifications.
  + The address can contain environment variables, each starting with a letter and consisting of `3` to `32` characters.

  Only letters, digits, hyphens (-), and underscores (_) are allowed in environment variables.

* `request_protocol` - (Required, String) Specifies the request protocol of the API.  
  The valid values are **HTTP**, **HTTPS** and **BOTH**.

* `security_authentication` - (Optional, String) Specifies the security authentication mode of the API request.  
  The valid values are **NONE**, **APP** and **IAM**, defaults to **NONE**.

* `simple_authentication` - (Optional, Bool) Specifies whether the authentication of the application code is enabled.  
  The application code must located in the header when `simple_authentication` is true.

* `authorizer_id` - (Optional, String) Specifies the ID of the authorizer to which the API request used.

* `request_params` - (Optional, List) Specifies the configurations of the front-end parameters.  
  The [object](#apig_api_request_params) structure is documented below.

* `backend_params` - (Optional, List) Specifies the configurations of the backend parameters.  
  The [object](#apig_api_backend_params) structure is documented below.

* `body_description` - (Optional, String) Specifies the description of the API request body, which can be an example
  request body, media type or parameters.  
  The request body does not exceed `20,480` characters.

* `cors` - (Optional, Bool) Specifies whether CORS is supported, defaults to **false**.

* `description` - (Optional, String) Specifies the API description.  
  The description contains a maximum of `255` characters and the angle brackets (< and >) are not allowed.

* `matching` - (Optional, String) Specifies the route matching mode.  
  The valid values are **Exact** and **Prefix**, defaults to **Exact**.

* `response_id` - (Optional, String) Specifies the APIG group response ID.

* `success_response` - (Optional, String) Specifies the example response for a successful request.  
  The response contains a maximum of `20,480` characters.

* `failure_response` - (Optional, String) Specifies the example response for a failure request.  
  The response contains a maximum of `20,480` characters.

* `mock` - (Optional, List, ForceNew) Specifies the mock backend details.  
  The [object](#apig_api_mock) structure is documented below.  
  Changing this will create a new API resource.

* `func_graph` - (Optional, List, ForceNew) Specifies the function graph backend details.  
  The [object](#apig_api_func_graph) structure is documented below.  
  Changing this will create a new API resource.

* `web` - (Optional, List, ForceNew) Specifies the web backend details.  
  The [object](#apig_api_web) structure is documented below. Changing this will create a new API resource.

* `mock_policy` - (Optional, List) Specifies the Mock policy backends.  
  The maximum blocks of the policy is 5.  
  The [object](#apig_api_mock_policy) structure is documented below.

* `func_graph_policy` - (Optional, List) Specifies the Mock policy backends.  
  The maximum blocks of the policy is 5.  
  The [object](#apig_api_func_graph_policy) structure is documented below.

* `web_policy` - (Optional, List) Specifies the example response for a failed request.  
  The maximum blocks of the policy is 5.  
  The [object](#apig_api_web_policy) structure is documented below.

<a name="apig_api_request_params"></a>
The `request_params` block supports:

* `name` - (Required, String) Specifies the request parameter name.  
  The valid length is limited from can contain `1` to `32`, only letters, digits, hyphens (-), underscores (_) and
  periods (.) are allowed.  
  If Location is specified as **HEADER** and `security_authentication` is specified as **APP**, the parameter name
  cannot be `Authorization` (case-insensitive) and cannot contain underscores.

* `required` - (Optional, Bool) Specifies whether the request parameter is required.

* `passthrough` - (Optional, Bool) Specifies whether to transparently transfer the parameter.

* `enumeration` - (Optional, String) Specifies the enumerated value(s).
  Use commas to separate multiple enumeration values, such as **VALUE_A,VALUE_B**.

* `location` - (Optional, String) Specifies the location of the request parameter.  
  The valid values are **PATH**, **QUERY** and **HEADER**, defaults to **PATH**.

* `type` - (Optional, String) Specifies the request parameter type.  
  The valid values are **STRING** and **NUMBER**, defaults to **STRING**.

* `maximum` - (Optional, Int) Specifies the maximum value or size of the request parameter.

* `minimum` - (Optional, Int) Specifies the minimum value or size of the request parameter.

-> For string type, The `maximum` and `minimum` means size. For number type, they means value.

* `example` - (Optional, String) Specifies the example value of the request parameter.  
  The example contains a maximum of `255` characters and the angle brackets (< and >) are not allowed.

* `default` - (Optional, String) Specifies the default value of the request parameter.
  The value contains a maximum of `255` characters and the angle brackets (< and >) are not allowed.

* `description` - (Optional, String) Specifies the description of the request parameter.  
  The description contains a maximum of `255` characters and the angle brackets (< and >) are not allowed.

<a name="apig_api_backend_params"></a>
The `backend_params` block supports:

* `type` - (Required, String) Specifies the backend parameter type.  
  The valid values are **REQUEST**, **CONSTANT** and **SYSTEM**.

* `name` - (Required, String) Specifies the backend parameter name, which contain of 1 to 32 characters and start with a
  letter. Only letters, digits, hyphens (-), underscores (_) and periods (.) are allowed. The parameter name is not
  case-sensitive. It cannot start with `x-apig-` or `x-sdk-` and cannot be `x-stage`. If the location is specified as
  **HEADER**, the name cannot contain underscores.

* `location` - (Required, String) Specifies the location of the backend parameter.  
  The valid values are **PATH**, **QUERY** and **HEADER**.

* `value` - (Required, String) Specifies the request parameter name corresponding to the back-end request parameter.

* `description` - (Optional, String) Specifies the description of the constant or system parameter.  
  The description contains a maximum of `255` characters and the angle brackets (< and >) are not allowed.

* `system_param_type` - (Optional, String) Specifies the type of the system parameter.  
  The valid values are **frontend**, **backend** and **internal**, defaults to **internal**.

<a name="apig_api_mock"></a>
The `mock` block supports:

* `response` - (Required, String) Specifies the response of the backend policy.  
  The description contains a maximum of `2,048` characters and the angle brackets (< and >) are not allowed.

  -> **NOTE:**  Mock enables APIG to return a response without sending the request to the backend. This is useful for
  testing APIs when the backend is not available.

* `authorizer_id` - (Optional, String) Specifies the ID of the backend custom authorization.

<a name="apig_api_func_graph"></a>
The `func_graph` block supports:

* `function_urn` - (Required, String) Specifies the URN of the FunctionGraph function.

* `version` - (Required, String) Specifies the function version.

* `timeout` - (Optional, Int) Specifies the timeout for API requests to backend service.  
  The valid value is range form `1` to `600,000`, defaults to `5,000`.

* `invocation_type` - (Optional, String) Specifies the invocation type.  
  The valid values are **async** and **sync**, defaults to **sync**.

* `authorizer_id` - (Optional, String) Specifies the ID of the backend custom authorization.

<a name="apig_api_web"></a>
The `web` block supports:

* `path` - (Required, String) Specifies the backend request address, which can contain a maximum of `512` characters and
  must comply with URI specifications.
  + The address can contain request parameters enclosed with brackets ({}).
  + The address can contain special characters, such as asterisks (*), percent signs (%), hyphens (-) and
    underscores (_) and must comply with URI specifications.
  + The address can contain environment variables, each starting with a letter and consisting of `3` to `32` characters.
    Only letters, digits, hyphens (-), and underscores (_) are allowed in environment variables.

* `host_header` - (Optional, String) Specifies the proxy host header.  
  The host header can be customized for requests to be forwarded to cloud servers through the VPC channel.  
  By default, the original host header of the request is used.

* `vpc_channel_id` - (Optional, String) Specifies the VPC channel ID. This parameter and `backend_address` are
  alternative.

* `backend_address` - (Optional, String) Specifies the backend service address.  
  The value which consists of a domain name or IP address, and a port number, with not more than `255` characters.  
  The backend service address must be in the format "{host name}:{Port number}", for example, `apig.example.com:7443`.  
  If the port number is not specified, the default HTTPS port `443`, or the default HTTP port `80` is used.  
  The backend service address can contain environment variables, each starting with a letter and consisting of `3` to
  `32` characters. Only letters, digits, hyphens (-), and underscores (_) are allowed.

* `request_method` - (Optional, String) Specifies the backend request method of the API.  
  The valid values are **GET**, **POST**, **PUT**, **DELETE**, **HEAD**, **PATCH**, **OPTIONS** and **ANY**.

* `request_protocol` - (Optional, String) Specifies the backend request protocol.  
  The valid values are **HTTP** and **HTTPS**, defaults to **HTTPS**.

* `timeout` - (Optional, Int) Specifies the timeout for API requests to backend service, the unit is **ms**.
  The valid value ranges from `1` to `600,000`, defaults to `5,000`.

* `retry_count` - (Optional, Int) Specifies the number of retry attempts to request the backend service.
  The valid value ranges from `-1` to `10`, defaults to `-1`.
  `-1` indicates that idempotent APIs will retry once and non-idempotent APIs will not retry.
  **POST** and **PATCH** are not-idempotent.
  **GET**, **HEAD**, **PUT**, **OPTIONS** and **DELETE** are idempotent.

  -> When the (web) backend uses the channel, the `retry_count` must be less than the number of available backend
     servers in the channel.

* `ssl_enable` - (Optional, Bool) Specifies whether to enable two-way authentication, defaults to **false**.

* `authorizer_id` - (Optional, String) Specifies the ID of the backend custom authorization.

<a name="apig_api_mock_policy"></a>
The `mock_policy` block supports:

* `name` - (Required, String) Specifies the backend policy name.  
  The valid length is limited from can contain `3` to `64`, only letters, digits and underscores (_) are allowed.

* `conditions` - (Required, List) Specifies an array of one or more policy conditions.  
  Up to five conditions can be set.
  The [object](#apig_api_conditions) structure is documented below.

* `response` - (Optional, String) Specifies the response of the backend policy.  
  The description contains a maximum of `2,048` characters and the angle brackets (< and >) are not allowed.

* `effective_mode` - (Optional, String) Specifies the effective mode of the backend policy.  
  The valid values are **ALL** and **ANY**, defaults to **ANY**.

* `backend_params` - (Optional, List) Specifies an array of one or more backend parameters.  
  The maximum of request parameters is `50`.  
  The [object](#apig_api_backend_params) structure is documented above.

* `authorizer_id` - (Optional, String) Specifies the ID of the backend custom authorization.

<a name="apig_api_func_graph_policy"></a>
The `func_graph_policy` block supports:

* `name` - (Required, String) Specifies the backend policy name.  
  The valid length is limited from can contain `3` to `64`, only letters, digits and underscores (_) are allowed.

* `function_urn` - (Required, String) Specifies the URN of the FunctionGraph function.

* `conditions` - (Required, List) Specifies an array of one or more policy conditions.  
  Up to five conditions can be set.
  The [object](#apig_api_conditions) structure is documented below.

* `invocation_mode` - (Optional, String) Specifies the invocation mode of the FunctionGraph function.  
  The valid values are **async** and **sync**, defaults to **sync**.

* `effective_mode` - (Optional, String) Specifies the effective mode of the backend policy.  
  The valid values are **ALL** and **ANY**, defaults to **ANY**.

* `timeout` - (Optional, Int) Specifies the timeout for API requests to backend service, the unit is `ms`.
  The valid value ranges from `1` to `600,000`, defaults to `5,000`.

* `version` - (Optional, String) Specifies the version of the FunctionGraph function.

* `backend_params` - (Optional, List) Specifies the configaiton list of the backend parameters.  
  The maximum of request parameters is `50`.  
  The [object](#apig_api_backend_params) structure is documented above.

* `authorizer_id` - (Optional, String) Specifies the ID of the backend custom authorization.

<a name="apig_api_web_policy"></a>
The `web_policy` block supports:

* `name` - (Required, String) Specifies the backend policy name.  
  The valid length is limited from can contain `3` to `64`, only letters, digits and underscores (_) are allowed.

* `path` - (Required, String) Specifies the backend request address, which can contain a maximum of `512` characters and
  must comply with URI specifications.  
  + The address can contain request parameters enclosed with brackets ({}).
  + The address can contain special characters, such as asterisks (*), percent signs (%), hyphens (-) and
      underscores (_) and must comply with URI specifications.
  + The address can contain environment variables, each starting with a letter and consisting of `3` to `32` characters.
    Only letters, digits, hyphens (-), and underscores (_) are allowed in environment variables.

* `request_method` - (Required, String) Specifies the backend request method of the API.  
  The valid types are **GET**, **POST**, **PUT**, **DELETE**, **HEAD**, **PATCH**, **OPTIONS** and **ANY**.

* `conditions` - (Required, List) Specifies an array of one or more policy conditions.  
  Up to five conditions can be set.  
  The [object](#apig_api_conditions) structure is documented below.

* `host_header` - (Optional, String) Specifies the proxy host header.  
  The host header can be customized for requests to be forwarded to cloud servers through the VPC channel.  
  By default, the original host header of the request is used.

* `vpc_channel_id` - (Optional, String) Specifies the VPC channel ID.  
  This parameter and `backend_address` are alternative.

* `backend_address` - (Optional, String) Specifies the backend service address.  
  The value which consists of a domain name or IP address, and a port number, with not more than `255` characters.  
  The backend service address must be in the format "{host name}:{Port number}", for example, `apig.example.com:7443`.  
  If the port number is not specified, the default HTTPS port `443`, or the default HTTP port `80` is used.  
  The backend service address can contain environment variables, each starting with a letter and consisting of `3` to
  `32` characters. Only letters, digits, hyphens (-), and underscores (_) are allowed.

* `request_protocol` - (Optional, String) Specifies the backend request protocol. The valid values are **HTTP** and
  **HTTPS**, defaults to **HTTPS**.

* `effective_mode` - (Optional, String) Specifies the effective mode of the backend policy. The valid values are **ALL**
  and **ANY**, defaults to **ANY**.

* `timeout` - (Optional, Int) Specifies the timeout, in ms, which allowed for APIG to request the backend service. The
  valid value is range from `1` to `600,000`, defaults to `5,000`.

* `retry_count` - (Optional, Int) Specifies the number of retry attempts to request the backend service.
  The valid value ranges from `-1` to `10`, defaults to `-1`.
  `-1` indicates that idempotent APIs will retry once and non-idempotent APIs will not retry.
  **POST** and **PATCH** are not-idempotent.
  **GET**, **HEAD**, **PUT**, **OPTIONS** and **DELETE** are idempotent.

  -> When the (web) backend uses the channel, the `retry_count` must be less than the number of available backend
     servers in the channel.

* `backend_params` - (Optional, List) Specifies an array of one or more backend parameters. The maximum of request
  parameters is 50. The [object](#apig_api_backend_params) structure is documented above.

* `authorizer_id` - (Optional, String) Specifies the ID of the backend custom authorization.

<a name="apig_api_conditions"></a>
The `conditions` block supports:

* `value` - (Required, String) Specifies the value of the backend policy.  
  For a condition with the input parameter source:
  + If the condition type is **Enumerated**, separate condition values with commas.
  + If the condition type is **Matching**, enter a regular expression compatible with PERL.

  For a condition with the Source IP address source, enter IPv4 addresses and separate them with commas. The CIDR
  address format is supported.

* `param_name` - (Optional, String) Specifies the request parameter name.
  This parameter is required if the policy type is **param**.

* `source` - (Optional, String) Specifies the backend policy type.  
  The valid values are **param** and **source**, defaults to **source**.

* `type` - (Optional, String) Specifies the condition type of the backend policy.  
  The valid values are **Equal**, **Enumerated** and **Matching**, defaults to **Equal**.

## Attribute Reference

In addition to all arguments above, the following attributes are exported:

* `id` - The API ID.
* `registered_at` - The registered time of the API.
* `updated_at` - The latest update time of the API.

## Import

APIs can be imported using their `name` and the related dedicated instance IDs, separated by a slash, e.g.

```shell
$ terraform import huaweicloud_apig_api.test <instance_id>/<name>
```
