```
Cài đặt nút đầy đủ Avail
Đây là hướng dẫn cài đặt Avail Node trên Ubuntu 22.04.
```
1.** **Cài đặt Rust****
```
sudo apt-get update
sudo apt install build-essential
sudo apt install --assume-yes git clang curl libssl-dev protobuf-compiler
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source ~/.cargo/env
rustup default stable
rustup update
rustup update nightly
rustup target add wasm32-unknown-unknown --toolchain nightly
```


2.**Sau khi đảm bảo bạn đã cài đặt Rust, bạn có thể chạy Avail Node bằng lệnh sau:**
```

git clone https://github.com/availproject/avail.git
cd avail
mkdir -p output
mkdir -p data
git checkout v1.7.2
cargo run --locked --release -- --chain kate -d ./output
```
**Lệnh này biên dịch và chạy Avail Node  được kết nối với Mạng Kate. **
```
2023-10-11 16:11:31 Avail Node    
2023-10-11 16:11:31 ✌️  version 1.7.0-ad024ff050e    
2023-10-11 16:11:31 ❤️  by Anonymous, 2017-2023    
2023-10-11 16:11:31 📋 Chain specification: Avail Kate Testnet    
2023-10-11 16:11:31 🏷  Node name: decorous-trade-0251    
2023-10-11 16:11:31 👤 Role: FULL    
2023-10-11 16:11:31 💾 Database: RocksDb at /tmp/substrateJwM8xd/chains/Avail Testnet_116d7474-0481-11ee-bc2a-7bfc086be54e/db/full    
2023-10-11 16:11:32 🔨 Initializing Genesis block/state (state: 0x6bc8…8ac6, header-hash: 0xd120…50c6)    
2023-10-11 16:11:32 👴 Loading GRANDPA authority set from genesis on what appears to be first startup.    
2023-10-11 16:11:33 👶 Creating empty BABE epoch changes on what appears to be first startup.    
2023-10-11 16:11:33 🏷  Local node identity is: 12D3KooWMmY2QLodvBGSiP1Cg9ysWrPSMN19qK3w35mRnUhq6pMX    
2023-10-11 16:11:33 Prometheus metrics extended with avail metrics    
2023-10-11 16:11:33 💻 Operating system: linux    
2023-10-11 16:11:33 💻 CPU architecture: x86_64    
2023-10-11 16:11:33 💻 Target environment: gnu    
2023-10-11 16:11:33 💻 CPU: 13th Gen Intel(R) Core(TM) i7-13700K    
2023-10-11 16:11:33 💻 CPU cores: 16    
2023-10-11 16:11:33 💻 Memory: 31863MB    
2023-10-11 16:11:33 💻 Kernel: 6.5.5-100.fc37.x86_64    
2023-10-11 16:11:33 💻 Linux distribution: Fedora Linux 37 (Workstation Edition)    
2023-10-11 16:11:33 💻 Virtual machine: no    
2023-10-11 16:11:33 📦 Highest known block at #0    
2023-10-11 16:11:33 〽️ Prometheus exporter started at 127.0.0.1:9615    
2023-10-11 16:11:33 Running JSON-RPC server: addr=127.0.0.1:9944, allowed origins=["http://localhost:*", "http://127.0.0.1:*", "https://localhost:*", "https://127.0.0.1:*", "https://polkadot.js.org"]    
2023-10-11 16:11:33 🏁 CPU score: 1.65 GiBs    
2023-10-11 16:11:33 🏁 Memory score: 19.49 GiBs    
2023-10-11 16:11:33 🏁 Disk score (seq. writes): 6.74 GiBs    
2023-10-11 16:11:33 🏁 Disk score (rand. writes): 2.65 GiBs    
2023-10-11 16:11:33 🔍 Discovered new external address for our node: /ip4/176.61.156.176/tcp/30333/ws/p2p/12D3KooWMmY2QLodvBGSiP1Cg9ysWrPSMN19qK3w35mRnUhq6pMX    
2023-10-11 16:11:34 [811] 💸 generated 9 npos targets    
2023-10-11 16:11:34 [811] 💸 generated 9 npos voters, 9 from validators and 0 nominators    
2023-10-11 16:11:34 [#811] 🗳  creating a snapshot with metadata SolutionOrSnapshotSize { voters: 9, targets: 9 }    
2023-10-11 16:11:34 [#811] 🗳  Starting phase Signed, round 1.
```

** Nhấn Ctrl + A + D để thoát khỏi màn hình **

3. **Tạo Systemd**
```

touch /etc/systemd/system/availd.service
nano /etc/systemd/system/availd.service
```

Thay đổi tên Trình xác thực của bạn và sao chép/dán nó
```

[Unit] 
Description=Avail Validator
After=network.target
StartLimitIntervalSec=0
[Service] 
User=root 
ExecStart= /root/avail/target/release/data-avail --base-path `pwd`/data --chain kate --name "boba"
Restart=always 
RestartSec=120
[Install] 
WantedBy=multi-user.target
```

**Để kích hoạt tính năng này để tự khởi động khi khởi động:
**
```

systemctl enable availd.service
```

Bắt đầu thủ công với:
```

systemctl start availd.service
```

Bạn có thể kiểm tra xem nó có hoạt động với:
```

systemctl status availd.service
```

Bạn có thể theo dõi nhật ký bằng:
```

journalctl -f -u availd
```

Kiểm tra nút của bạn trên https://telemetry.avail.tools/

<img width="783" alt="Screenshot 2023-10-28 155757" src="https://github.com/boba3189/Install-Avail-Full-Node/assets/15023945/5d36047b-0516-4141-9b7d-d2938b097a70">


