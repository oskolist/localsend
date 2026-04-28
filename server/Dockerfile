FROM rust:1.83 AS builder
WORKDIR /app

# 1. Copy the 'core' dependency first so the relative path "../core" resolves correctly
# inside the container structure.
COPY core ./core

# 2. Copy server manifest files into the server subdirectory
COPY server/Cargo.* ./server/

# 3. Set working directory to match the location of Cargo.toml
WORKDIR /app/server

# 4. Compile Dependencies (Dummy build for caching)
RUN mkdir -p src && echo 'fn main() { panic!("Dummy Image Called!") }' > ./src/main.rs
RUN cargo build --release

# 5. Add Source
COPY server/src ./src

# 6. Break cargo cache to force recompilation of actual code
RUN touch ./src/main.rs

# 7. Compile Final Binary
RUN cargo build --release

FROM debian:bookworm-slim AS runtime
# Note: The binary path now includes /server/ because of the monorepo structure
COPY --from=builder /app/server/target/release/server /server
EXPOSE 3000
ENTRYPOINT ["/server"]
