# 톤메이트 (ToneMate) — 상황·상대별 한국어 메시지 빌더 MCP

카카오톡에서 "이거 상사한테 보낼 건데 정중하게 고쳐줘" 한마디로
상대·의도에 맞는 메시지 3버전을 받는 MCP 서버.

## 왜 다른가 (차별화 포인트)
- PlayMCP 등록 서버 중 글쓰기/톤 조절류 **없음** (선점).
- GitHub의 한국어 MCP는 '맞춤법 검사'만 → 톤메이트는 '상대별 톤 + 선택지'.
- 맨몸 LLM 즉흥 교정과 달리 상대별 규칙(호칭/어미/금기/톤)을 구조화해 **일관성** 확보.
- 외부 데이터·API·저장소 **불필요** → 안정성·보안 심사 유리, 호출량 부담 0.

## tool 4개
- polish_message(text, relation, intent) : 3버전(정중/간결/사유강조)
- soften_message(text, relation)         : 화난 메시지 식히기 + 뉘앙스 경고
- reply_helper(received, relation, stance): 답장 후보 3개
- check_korean(text)                      : 맞춤법·띄어쓰기 (부가)

relation: 상사 / 거래처 / 친구 / 연인 / 가족
intent  : 거절 / 사과 / 부탁 / 독촉 / 감사 / 보고 / 일반

## 로컬 테스트
pip install -r requirements.txt
TRANSPORT=stdio python server.py     # stdio 모드
python server.py                     # HTTP 모드 (기본, PORT=8000)

## 배포 → PlayMCP 등록 절차
1. 카카오 클라우드에 MCP 서버 Endpoint 생성 (가이드: kko.to/player10)
   - Docker 이미지로 배포하거나, server.py를 직접 구동 (HTTP, 0.0.0.0:$PORT)
   - 엔드포인트 경로: https://<host>/mcp
2. PlayMCP 개발자 콘솔 → '새로운 MCP 서버 등록' → 위 엔드포인트 입력
3. '임시 등록'으로 PlayMCP에서 테스트
4. 완성되면 '등록 및 심사 요청' (영업일 최대 7일, 7/7 전 제출 권장)
5. 심사 통과 후 공개 상태를 **'전체 공개'**로 변경 (필수!)
6. 공모전 소개 페이지 → [Player 예선 참여] 버튼으로 최종 응모 (1회만)

## 해자 강화 (시간 나면)
RELATION_RULES / INTENT_RULES에 한국 비즈니스 화법 노하우를 계속 추가.
이 규칙 테이블이 정교할수록 맨몸 카나나와 격차가 벌어진다.
