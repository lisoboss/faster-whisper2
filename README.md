# faster-whisper2

## CUDA Runtime

如果使用 NVIDIA GPU，请确保 CUDA 运行时库可以被动态链接器找到。

### Ubuntu（系统安装 CUDA）

```bash
export LD_LIBRARY_PATH=/usr/local/cuda/lib64:${LD_LIBRARY_PATH}
```

如果 CUDA 安装在其他目录，请修改为对应路径。

### 使用 PyPI/uv 安装 NVIDIA Runtime

```bash
export LD_LIBRARY_PATH="$(
python - <<'PY'
import site
from pathlib import Path

dirs = []

for root in map(Path, site.getsitepackages()):
    nvidia = root / "nvidia"
    if not nvidia.exists():
        continue

    dirs.extend(
        str(p) for p in nvidia.glob("cu*/lib") if p.is_dir()
    )

    for name in ("cudnn", "cusparselt"):
        p = nvidia / name / "lib"
        if p.is_dir():
            dirs.append(str(p))

print(":".join(dict.fromkeys(dirs)))
PY
):${LD_LIBRARY_PATH}"
```