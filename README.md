# onward-catalog

The requirement lists served to the **Onward** app — which documents a given
procedure asks for, how long each one typically takes to obtain, and how long it
stays valid.
**Onward** 앱이 받아가는 요건 데이터입니다 — 어떤 절차에 어떤 서류가 필요한지,
발급에 보통 얼마나 걸리는지, 얼마나 유효한지.

**This repository is build output. Nothing here is edited by hand.** It is written
by a GitHub Action from the app's source repository, which is where the data is
reviewed and where changes are made.
**이 저장소는 빌드 결과물이고, 손으로 수정하지 않습니다.** 앱 소스 저장소에서
GitHub Action이 생성합니다.

## Not legal advice / 법률 자문이 아닙니다

General information about commonly requested documents. **It is not legal advice,
and your own requirements may differ** — by your circumstances, your state, your
consulate, and by the date you read this. Always confirm against the official page
linked from each entry.
일반적인 정보일 뿐 **법률 자문이 아니며, 본인의 요건은 다를 수 있습니다.** 각 항목에
연결된 공식 페이지에서 반드시 직접 확인하세요.

Entries marked `"draft": true` have not been checked against their official source.
`"draft": true`인 항목은 공식 출처와 대조되지 않았습니다.

## Layout

```
catalog/
  latest.json        the pointer — small, stable name, the only file a client polls
  <version>.json     one build, named by its own content hash
```

Every published build stays. `latest.json` moves; the hashed files do not, so a
client that read the pointer a moment ago still finds the file it was promised.
게시된 빌드는 전부 남습니다. `latest.json`만 움직입니다.

## The pointer

`catalog/latest.json`:

```json
{
  "version": "b832b9548245",
  "publishedAt": "2026-09-04",
  "url": "https://sungle7890.github.io/onward-catalog/catalog/b832b9548245.json",
  "sha256": "65f3c2c44f4e92d4fe75a010316dbf3d65c0f79911cac3b6547993034fb90cab",
  "bytes": 51193
}
```

**`version` is a hash of the content, not a sequence.** The same data always
produces the same version, so an unchanged catalog is never re-downloaded and
changed data can never appear under an old version.
**`version`은 내용의 해시이지 일련번호가 아닙니다.**

**Which means versions cannot be ordered.** A hash says *different*, never *newer*.
Compare `publishedAt` to decide whether a build is worth taking — a client that
compares versions instead will happily "update" backwards.
**따라서 버전으로는 순서를 알 수 없습니다.** 최신 여부는 `publishedAt`으로 판단하세요.

## If you are reading this to consume it

It is public data on a static host, so you are welcome to. Four checks are worth
copying, because the app does all four and each one is there for a reason:

1. **`url` is on this origin and is `https`.** A pointer that can redirect
   anywhere is a pointer that can serve anything.
2. **The body hashes to `sha256`.** Verify before parsing, not after.
3. **The body's own `version` equals the pointer's.** They are written by one
   build; disagreement means something is being assembled from two.
4. **`publishedAt` is newer than what you already hold.** See above.

Then validate the shape before trusting a single field. This is data that arrived
over a network, whoever served it.
네트워크로 받은 데이터입니다 — 누가 서빙했든, 파싱 전에 검증하세요.

## No API

Static files. No database, no authentication, no per-user state, and no request
carries an identifier — the app fetches `latest.json` and nothing else. There is
nothing here that knows who asked.
정적 파일뿐입니다. API도, 인증도, 사용자별 상태도 없습니다.

## Issues

Corrections to the data are welcome as issues on this repository. **Pull requests
here cannot be merged** — the next publish would overwrite them. The fix has to
happen in the source repository, and an issue is how it gets there.
데이터 오류 제보는 이 저장소의 이슈로 받습니다. **여기로 보낸 PR은 머지할 수
없습니다** — 다음 게시 때 덮어써집니다.
