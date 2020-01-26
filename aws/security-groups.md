# Security Group
- They are associated with compute instance or resources.
- Their default behaviour is to **`deny all inbound traffic`**.
- They have to explicitly allow traffic using rules based on **`port, protocol, and ip-range`**
- They are **`stateful`**, which means that any packet that is sent out is remembered and its destination is allowed to send traffic back in, even if by default all traffic is blocked.