# Key Points of SDK Access {#concept_x4q_ms2_xdb .concept}

Key Points of SDK Access.

## 1. Obtain the Game Shield node IP and port {#section_pq5_ns2_xdb .section}

The client must call the SDK's initialization interface as early as possible. We recommend putting it at the head of all service logic.

The changes in the process before accessing the original server are as follows:

-   Original: The client obtains the server IP and port -\> connects the server
-   New: The client calls the SDK to get the Game Shield IP and port -\> connects the Game Shield IP and port -\> the server

The IP and port returned by Game Shield must be fully used. The IP and port instances returned by Game Shield are as follows:

-   Normal mode: 116.211.171.31 10009
-   Link encryption mode: 127.0.0.1 56382 \([How to be immune to connection flood attacks](../../../../../reseller.en-US/User Guide/Encrypted Tunnel Mode.md#)\)

Normal mode returns public IP addresses and fixed ports \(with the same forwarding rule configuration\), while link encryption mode returns local addresses and random ports.

## 2. Optimization of the logic for the client to reconnect after losing connection {#section_sq5_ns2_xdb .section}

When the client times out in the heartbeat or connection, it usually needs to immediately enter the reconnection logic. We suggest that the time-out period be 3 to 5 seconds \(referring to the time for issuing cards in a game\), and prompt the user of connection failure when reconnection exceeds 3 to 5 times or when the reconnection time exceeds 60 seconds. This may weaken the user's perception of reconnection.

**The logic of communications between the client and the server must be that a data request is first initiated by the client before the server responds, otherwise it is blocked by the Game Security Gateway cluster of Game Shield as an null connection attack.**

## 3. Server-side optimization and transformation {#section_tq5_ns2_xdb .section}

The user’s online status must be determined to set the aging time \(usually 15 to 30 seconds\).

The user’s re-login must be processed such that the new login user kicks out the old logged-in user \(the reconnection may be faster than the aging time\).

To obtain the real client IP, see [Obtain real client IP](../../../../../reseller.en-US/User Guide/Obtain Client Real IP.md#).

## 4. IPV6 support {#section_uq5_ns2_xdb .section}

Game Shield supports the IPv6 environment. During the iOS audit, Game Shield automatically determines the network environment to enable the IPv6 dispatching and forwarding environment.

## 5. Downgrade when the Game Shield service is unavailable {#section_vq5_ns2_xdb .section}

The client must downgrade when Game Shield calls the SDK and returns a non-zero code. The usual practice is to hard code the Anti-anti-DDoS node of Game Shield as the backup logic, and use the Anti-DDoS Pro service as the backup service when the Game Shield service is unavailable.

Whether Game Shield is enabled or not, a switch must be set on the server side to control the flow of data.

## 6. Suggestions for user grouping and Game Shield node grouping {#section_wq5_ns2_xdb .section}

We recommend that your server also implement stratified governance on your clients. Different users correspond to different Game Shield groups. The general logic is as follows:

-   New players are those whose registration time is less than 10 days
-   Old players are those whose registration time is below 10 days, whose game time is below or equal to 10 hours, or who have played below 30 games
-   VIP players are those whose game time is over 10 hours, who play over 30 games, or who have spent over 100 rmb in the game

The preceding logic is for reference only. You can modify it according to your service situation. We recommend that you update the logic of user grouping regularly.

**The service can assign custom users to a designated Game Shield group and has the logic for manually dispatching malicious users once they are detected.**

## 7. Check before officially taking the client online {#section_yq5_ns2_xdb .section}

The following must be checked to ensure the protective effect of Game Shield after access:

1.  Game Shield's on-off switch.
2.  Scheduling logic and scheduling time of nodes in Game Shield groups after the nodes are put into the black hole routing status \(contact the Game Shield team to simulate attacks\).
3.  Scheduling logic for when the Game Shield SDK fails to obtain the node \(contact the Game Shield team to simulate attacks\).
4.  Manual scheduling function of Game Shield. \( Non-mandatory, and this can be implemented through smart scheduling of Game Shield \)
5.  Game Shield SDK's compatibility after integration \(provide APK and IOS packages to the Game Shield team for stability and compatibility test\).

