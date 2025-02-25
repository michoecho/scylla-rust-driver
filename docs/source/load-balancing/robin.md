# Round robin
The simplest load balancing policy available.\
Takes all nodes in the cluster and uses them one after another.\

For example if there are nodes `A`, `B`, `C` in the cluster,
this policy will use `A`, `B`, `C`, `A`, `B`, ...

### Example
To use this policy in `Session`:
```rust
# extern crate scylla;
# use scylla::Session;
# use std::error::Error;
# async fn check_only_compiles() -> Result<(), Box<dyn Error>> {
use scylla::{Session, SessionBuilder};
use scylla::transport::ExecutionProfile;
use scylla::transport::load_balancing::RoundRobinPolicy;
use std::sync::Arc;

let handle = ExecutionProfile::builder()
    .load_balancing_policy(Arc::new(RoundRobinPolicy::new()))
    .build()
    .into_handle();

let session: Session = SessionBuilder::new()
    .known_node("127.0.0.1:9042")
    .default_execution_profile_handle(handle)
    .build()
    .await?;
# Ok(())
# }
```