# Network Access Control List (NACL)
- These are associated with subnets.
- They are **`stateless (they don't remember)`**, which means that the response to any packet sent out must be allowed by explicit ACL rules, otherwise it will be blocked.
- NACL can be associated with 1 or more subnets.
- > The Default NACL allow all inbound and outbound traffic.
- > The new NACL that we make denies all traffic by default and we have to explicity defines new rules.
- The **`*`** named rule is called **`catch all`** and we cannot modify or delete it.
- Rules are evaluated based on **Rule #** from **`lowest to heighest`**.
- The 1st rule evaluated that applies to the traffic type, gets immediately applied and executed regardless of the rules that come after `(have a higher rule #)`.