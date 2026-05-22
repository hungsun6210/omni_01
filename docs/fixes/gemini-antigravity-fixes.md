# Gemini & Antigravity Fixes

## 1. Antigravity OAuth — `platform` 필드 제거

**파일:** `open-sse/services/antigravityHeaders.ts`

Google API가 `platform: "WINDOWS"` 값을 거부하여 400 에러 발생.
`getAntigravityLoadCodeAssistMetadata()`에서 `platform` 필드를 제거함.

## 2. Gemini API — baseUrl 오류 수정 (검증)

**파일:** `open-sse/config/providerRegistry.ts`

- **변경 전:** `baseUrl: "https://generativelanguage.googleapis.com/v1beta/models"` + `urlBuilder: \`${base}/${model}:${action}\``
  - 검증 URL: `https://.../v1beta/models/models?key=...` (더블 `/models`) → **404**
- **변경 후:** `baseUrl: "https://generativelanguage.googleapis.com/v1beta"` + `urlBuilder: \`${base}/models/${model}:${action}\``
  - 검증 URL: `https://.../v1beta/models?key=...` → **200**

## 3. Gemini "Import from Models" 버튼 숨김 해제

**파일:** `src/app/(dashboard)/dashboard/providers/[id]/page.tsx:3188`

Gemini에서 버튼이 `null`로 강제 숨겨져 있어 모델 동기화 불가.
`providerId === "gemini" ? null` 가드 제거.

## 4. Gemini API 호출 URL에 `/models/` 누락 수정

**파일:** `open-sse/executors/default.ts:229`

- **변경 전:** `${baseUrl}/gemini-2.0-flash:generateContent` → **404**
- **변경 후:** `${baseUrl}/models/gemini-2.0-flash:generateContent` → **200**

DefaultExecutor.buildUrl에서 Gemini URL에 `/models/` 경로가 빠져 있었음.
