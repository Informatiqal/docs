# FunkSSE

!!! warning "WARNING"

    Not a real product ... yet. Basic codebase is in place but nothing in a complete state

[Qlik SSE](https://help.qlik.com/en-US/sense-developer/May2025/Content/Sense_Helpsites/SSE-open-source.htm) is considered "dead end" from Qlik. But SSE is still very potent protocol to enhance Qlik with custom functionality.

SSE protocol can be used to create custom function that can be used in the load script and in the frontend. Rob Wunderlich created few functions in the past that can give an example what can be build with SSE - [qcb-qlik-sse](https://github.com/RobWunderlich/qcb-qlik-sse/blob/master/docs/functions.md)

`FunkSEE` is (will be) an attempt to create a backend server that will server user defined functions. It's main purpose is to offload the server creation and allow the developers to create their functions. The server will load these functions which the Qlik developers will be able to use in Qlik itself.

In the future I'll create a separate repo where I'll create some base functions and where users/developers can submit their requests for future functions. But that repo will be separate from `FunkSSE`.
 