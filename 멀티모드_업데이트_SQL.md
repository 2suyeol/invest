# 멀티 모드 업데이트 — Supabase SQL (코인·대출)

이번 수정으로 참가자 행에 **코인**과 **대출** 정보가 추가됐습니다.
아래 SQL을 **한 번** 실행해야 매매·뉴스 구매·대출이 정상 작동합니다.

Supabase → **SQL Editor → New query** 에 붙여넣고 **Run**:

```sql
alter table public.room_players
  add column if not exists coins          int     not null default 5,
  add column if not exists loan_principal numeric not null default 0,
  add column if not exists loan_interest  numeric not null default 0,
  add column if not exists bought         jsonb   not null default '{}'::jsonb;
```

- `coins`: 시작 시 5개. 코인으로 뉴스를 단계별로 구매(1단계 1개, 2단계 3개, 3단계 7개).
- `loan_principal` / `loan_interest`: 대출 원금 / 누적 이자.
- `bought`: 이번 라운드에 구매한 뉴스 목록(본인만 내용 확인).

> `column ... already exists`가 떠도 무시하면 됩니다. (이미 적용된 것)

이전 단계(2-A, 2-B) SQL을 아직 안 했다면 그것부터 실행하세요:
- `멀티모드_2A_가이드.md` (방/참가자/실시간)
- `멀티모드_2B_가이드.md` (게임 상태·지갑 컬럼·rp_update 권한)
