# Drone Detector Service

> 把训练好的无人机检测模型，变成**任何人都能调用的 HTTP 服务**。
> 状态：🚧 规划中（2026-11 立项，与 FastAPI/Docker 学习同步推进）

## 这个仓库是什么

大部分计算机视觉项目停在 `model.train()` 打印出 mAP 就结束了——模型只能在自己电脑上用。本仓库的目标是补上后半程：

```
训练好的 best.pt  →  FastAPI 推理服务  →  Docker 镜像  →  任何程序都能调用
```

模型训练在主仓库 [uav-vision-radar](https://github.com/len536/uav-vision-radar) 完成；本仓库只关心**部署与调用**。

## 规划的接口设计

| 端点 | 方法 | 入参 | 出参 |
|---|---|---|---|
| `/detect` | POST | 一张图片（multipart） | JSON：`[{cls, conf, x1, y1, x2, y2}, ...]` |
| `/health` | GET | — | 服务存活 + 模型加载状态 |
| `/docs` | GET | — | FastAPI 自带的交互式文档（Swagger UI） |

## 技术栈

- **推理**：ultralytics YOLOv8（GPU: RTX 5060 / CPU 兜底）
- **服务**：FastAPI + uvicorn
- **交付**：Dockerfile（`docker run -p 8000:8000` 一行起服务）
- **客户端**：PyQt5 上位机通过 HTTP 调用，模型与界面解耦

## 快速开始（建设中）

```bash
# 目标形态（尚未实现）：
docker run -p 8000:8000 len536/drone-detector
curl -F "file=@drone.jpg" http://localhost:8000/detect
```

## 路线图

- [ ] v0.1 — 最小 `/detect` 接口（单帧推理，返回 JSON）
- [ ] v0.2 — 并发与批处理，延迟压测（单帧耗时写入 README）
- [ ] v0.3 — Dockerfile + 使用文档
- [ ] v0.4 — PyQt 客户端接入（界面代码在主仓库 `ui/`）
- [ ] v1.0 — Swagger 文档截图 + 完整部署说明
