# SDK error codes

This topic describes the common error codes defined by the GameShield SDK.

## Overview

|Error code|Description|
|----------|-----------|
|0|No error occurred.|
|1000-1999|An error code returned because a network communication failure occurred.|
|2000-2999|An error code returned because the appkey parameter fails the verification or the initialization of the GameShield SDK fails when you attempt to integrate the GameShield SDK.|
|3000-3999|An error code returned because an error occurred in the GameShield server when you integrate the GameShield SDK.|
|4000-4999|An error code returned because an error occurred during the data exchange between the GameShield SDK and the GameShield server.|

## Common error codes

|Error code|Description|Solution|
|----------|-----------|--------|
|-1|The error code returned because the group name \(groupname\) or another parameter was not set.|Enter a valid value.|
|0|No error occurred.|N/A.|
|2000|The error code returned because the appkey parameter was not set.|Enter a valid value.|
|2001|The error code returned because the value of the appkey parameter is in an invalid format.|Use a valid format.|
|2002|The error code returned because the value of the appkey parameter exceeds the maximum length.|Check that the value of the appkey parameter does not exceed the maximum length.|
|2005|The error code returned because the API operation that is used to initialize the GameShield SDK was not called.|Call the initialization API operation first.|
|3201|The error code returned because the Gameshield SDK is not enabled.|Enable the Gameshield SDK.|
|3305|The error code returned because the SDK request parameters are invalid.|Check that the SDK request parameters are valid. If the issue persists, contact GameShield technical support.|
|3306|The error code returned because the SDK request type is invalid.|Check that the specified API operation is correct. If the issue persists, contact GameShield technical support.|
|3307|The error code returned because the SDK request parameters are invalid.|Check that the SDK request parameters are valid. If the issue persists, contact GameShield technical support.|
|3500|The error code returned because no IP addresses are configured in the specified group.|Add IP addresses to the specified group in the GameShield console.|
|3600|The error code returned because no IP addresses are available in the specified group.|Add IP addresses to the specified group or enable unlimited protection against DDoS.|
|3700|The error code returned because the value of the groupname parameter is invalid.|Enter a valid value. If the issue persists, contact GameShield technical support.|
|3702|The error code returned because the protection target was not specified.|Set a protection target for unlimited protection against DDoS attacks in the GameShield console.|
|3703|The error code returned because the forwarding rule was not specified.|Set a port for unlimited protection against DDoS attacks in the GameShield console.|
|3800|The error code returned because SDK data was hijacked when it was transmitted over HTTP connections.|Contact GameShield technical support.|
|3999|The error code returned because the API parameter of the GameShield SDK is invalid.|Check that the API operation parameter is valid. If the issue persists, contact GameShield technical support.|
|4000|The error code returned because SDK data was hijacked when it was transmitted over HTTP connections.|Contact GameShield technical support.|
|9100|The error code returned because the API of the Gameshield SDK received simultaneous calls from multiple threads.|Call the API from one thread at a time.|

**Note:** If the issue persists, contact GameShield technical support.

