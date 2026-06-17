Python `zipfile` 로 패키징한다(zip CLI 미설치 가능):
```python
import zipfile, os
src = "output/it-support-insights-v2"
dst = "output/it-support-insights-v2.zip"
if os.path.exists(dst): os.remove(dst)
with zipfile.ZipFile(dst, "w", zipfile.ZIP_DEFLATED) as z:
    for root, _, files in os.walk(src):
        for f in files:
            full = os.path.join(root, f)
            arc = os.path.relpath(full, src)   # 경로는 패키지 루트 기준 상대경로
            z.write(full, arc)
```
검증: `manifest.json` 이 ZIP **루트**에 있어야 한다(하위 폴더 안이면 안 됨).
주의: OneDrive 폴더에서 직접 ZIP/OOXML 생성 시 손상될 수 있으니, 필요하면 로컬 임시 폴더에서
생성 후 복사한다.
