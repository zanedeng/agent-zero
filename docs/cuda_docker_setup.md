# Agent Zero：CUDA GPU 支持 🚀

本指南解释了如何使用 CUDA 构建和运行支持 NVIDIA GPU 加速的 Agent Zero。通过 CUDA 运行可以利用 GPU 实现 AI 工作负载的更快性能。

---

## 前提条件

在开始之前，请确保您具备：

1. 具有 CUDA 功能的 **NVIDIA GPU**
2. 在主机系统上安装的 **NVIDIA 驱动程序**
3. **NVIDIA Container Toolkit**（[安装指南](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html)）  
   _这使 Docker 能够访问您的 GPU_
4. 已安装 **Docker** 和 **Docker Compose**

---

## 1. 构建 CUDA Docker 镜像

在此目录中打开终端并运行：

```bash
# 设置要构建的分支（默认：main）
$branch="main"
docker build --no-cache -t frdel/agent-zero-run-cuda:testing --build-arg BRANCH=$branch -f Dockerfile.cuda .
```

---

## 2. 使用 CUDA 支持运行 Agent Zero

您可以使用 Docker Compose 启动支持 GPU 的 Agent Zero：

```bash
# 在 Linux、macOS 或 Windows PowerShell 上：
docker-compose -f docker-compose.cuda.yml up -d
```

- 这将在后台启动启用了 GPU 加速的 Agent Zero。

---

## 3. 访问 Agent Zero

容器运行后，打开浏览器并访问：

[http://localhost:50080](http://localhost:50080)

---

## 4. 停止 Agent Zero

要停止启用 CUDA 的 Agent Zero 容器：

```bash
docker-compose -f docker-compose.cuda.yml down
```

---

## 5. 在 CPU 和 GPU 版本之间切换

您可以轻松地在 CPU 和 GPU 版本之间切换：

1. **停止当前运行的版本：**
   ```bash
   # CPU 版本：
   docker-compose down
   # GPU 版本：
   docker-compose -f docker-compose.cuda.yml down
   ```

2. **启动您想要的版本：**
   ```bash
   # CPU 版本：
   docker-compose -f docker-compose.yml up -d

   # GPU（CUDA）版本：
   docker-compose -f docker-compose.cuda.yml up -d
   ```

---

## 故障排除和提示

- **首次设置可能需要几分钟**，因为需要下载和安装依赖项。
- 如果遇到 GPU 访问问题，请验证您的 NVIDIA 驱动程序和 NVIDIA Container Toolkit 是否正确安装。
- 要检查容器内是否可用 CUDA，您可以运行：
  ```bash
  docker exec -it <container_name> python3 -c "import torch; print(torch.cuda.is_available())"
  ```
- 有关高级配置，请参阅 [`Dockerfile.cuda`](mdc:docker/run/Dockerfile.cuda) 中的注释。

---

## 更多信息

- [NVIDIA Container Toolkit 文档](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html)
- [Agent Zero 项目](https://github.com/frdel/agent-zero)（如果不同，请替换为您的实际仓库链接）

---

**享受 Agent Zero 和 CUDA 带来的加速 AI 体验！**
