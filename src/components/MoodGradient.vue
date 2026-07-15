<script setup>
// 무드별 그라디언트 애니메이션 (6종) — 무드마다 색뿐 아니라 "모션 메커니즘"이 다르다.
// 사용: <MoodGradient :mood-id="mood.id" />
// 부모를 가득 채우므로(absolute inset:0) 쓰는 쪽에서 position:relative + 크기를 잡아준다.
//
//  고요(serene)        안개 + 수면 위 파문 — 느리게 퍼지는 동심원
//  사색(contemplative)  보랏빛 실크 물결 — 분홍 빛무리가 수평으로 느리게 흐름 (레퍼런스 기반)
//  활력(vital)          맞물려 도는 주황 구체 쌍 + 심장박동 (레퍼런스 기반)
//  축제(festive)        보케 불빛 — 흩어진 광점들이 엇박으로 점멸
//  미식(gourmet)        접시에서 피어오르는 김 + 캔들 플리커
//  도심 산책(citywalk)  패럴랙스 쇼윈도 — 두 겹의 블록이 다른 속도로 지나감
defineProps({
  moodId: { type: String, required: true },
})
</script>

<template>
  <div class="mg" :class="`mg--${moodId}`" aria-hidden="true">
    <span class="l1" />
    <span class="l2" />
    <span class="l3" />
  </div>
</template>

<style scoped>
.mg {
  position: absolute;
  inset: 0;
  overflow: hidden;
}

.mg > span {
  position: absolute;
  will-change: transform, opacity;
}

/* ============ 고요 — 새벽 수면 위 안개 ============
   기준 디자인(34s/46s/12s)에서 속도만 살짝 올린 버전 (22s/30s/10s) */
.mg--serene {
  background: linear-gradient(170deg, #d9ecf5 0%, #8fc0da 38%, #6aa9c9 62%, #3c6a85 100%);
}

.mg--serene .l1 {
  width: 70%;
  height: 70%;
  left: -12%;
  top: -18%;
  border-radius: 50%;
  filter: blur(46px);
  background: radial-gradient(circle, rgba(235, 247, 252, 0.9), rgba(235, 247, 252, 0) 70%);
  animation: sr-mist 22s ease-in-out infinite alternate;
}

.mg--serene .l2 {
  width: 80%;
  height: 80%;
  right: -18%;
  bottom: -28%;
  border-radius: 50%;
  filter: blur(46px);
  background: radial-gradient(circle, rgba(38, 79, 104, 0.75), rgba(38, 79, 104, 0) 70%);
  animation: sr-deep 30s ease-in-out infinite alternate;
}

.mg--serene .l3 {
  inset: 0;
  background: rgba(255, 255, 255, 0.05);
  animation: sr-breath 10s ease-in-out infinite;
}

@keyframes sr-mist {
  from {
    transform: translate(0, 0) scale(1);
  }
  to {
    transform: translate(16%, 10%) scale(1.15);
  }
}

@keyframes sr-deep {
  from {
    transform: translate(0, 0) scale(1.1);
  }
  to {
    transform: translate(-12%, -8%) scale(1);
  }
}

@keyframes sr-breath {
  0%,
  100% {
    opacity: 0;
  }
  50% {
    opacity: 1;
  }
}

/* ============ 사색 — 보랏빛 실크 물결 ============
   레퍼런스: 청보라 필드 위로 분홍 빛무리가 수평으로 흐르는, 실크처럼 뭉개진 결 */
.mg--contemplative {
  background: linear-gradient(165deg, #4a2ee2 0%, #8a5ce8 35%, #c26ff0 70%, #ee93f2 100%);
}

/* 분홍 빛무리: 납작한 타원 여러 개가 한 겹으로, 아주 천천히 옆으로 흐른다 */
.mg--contemplative .l1 {
  inset: -18% -40%;
  filter: blur(26px);
  background:
    radial-gradient(55% 16% at 30% 36%, rgba(255, 170, 245, 0.75), transparent 70%),
    radial-gradient(60% 14% at 68% 60%, rgba(255, 150, 240, 0.6), transparent 70%),
    radial-gradient(50% 12% at 24% 82%, rgba(255, 185, 250, 0.65), transparent 70%);
  animation: ct-silk-a 18s ease-in-out infinite alternate;
}

/* 깊은 청보라 그늘: 반대 방향으로 더 느리게 — 결이 계속 어긋난다 */
.mg--contemplative .l2 {
  inset: -18% -40%;
  filter: blur(30px);
  background:
    radial-gradient(60% 20% at 76% 14%, rgba(43, 22, 205, 0.7), transparent 70%),
    radial-gradient(45% 14% at 58% 44%, rgba(70, 40, 212, 0.45), transparent 70%);
  animation: ct-silk-b 26s ease-in-out infinite alternate;
}

.mg--contemplative .l3 {
  inset: 0;
  background: rgba(255, 255, 255, 0.05);
  animation: ct-breath 12s ease-in-out infinite;
}

@keyframes ct-silk-a {
  to {
    transform: translate(9%, -3%);
  }
}

@keyframes ct-silk-b {
  from {
    transform: translate(4%, 0);
  }
  to {
    transform: translate(-7%, 3%);
  }
}

@keyframes ct-breath {
  0%,
  100% {
    opacity: 0;
  }
  50% {
    opacity: 1;
  }
}

/* ============ 활력 — 맞물려 도는 두 개의 심장 ============
   레퍼런스: 마젠타 필드 위에서 크림 심지를 품은 주황 구체 쌍이 서로 꼬이며 도는 구도 */
.mg--vital {
  background: radial-gradient(140% 140% at 42% 38%, #ff9a4a 0%, #fb6b82 48%, #ee3ec0 78%, #d81fd2 100%);
}

/* 구체 쌍: 한 레이어에 대각선으로 두 개를 그리고 레이어째 회전 → 서로 쫓아 도는 궤도 */
.mg--vital .l1 {
  inset: -28%;
  filter: blur(18px);
  background:
    radial-gradient(circle at 28% 26%, rgba(255, 237, 201, 0.95) 0%, rgba(255, 178, 94, 0.85) 16%, rgba(255, 116, 51, 0.6) 34%, rgba(250, 90, 120, 0.3) 55%, transparent 78%),
    radial-gradient(circle at 72% 74%, rgba(255, 227, 184, 0.9) 0%, rgba(255, 162, 81, 0.8) 18%, rgba(255, 109, 46, 0.55) 36%, rgba(245, 80, 140, 0.28) 58%, transparent 80%);
  animation: vt-orbit 9s linear infinite;
}

/* 구체 사이를 가르는 밝은 경계광 (레퍼런스의 노란 빛띠) — 반대 방향으로 회전 */
.mg--vital .l2 {
  inset: -28%;
  background: linear-gradient(45deg, transparent 30%, rgba(255, 240, 205, 0.35) 50%, transparent 70%);
  filter: blur(30px);
  animation: vt-orbit-rev 9s linear infinite;
}

/* 심장박동: 빠르고 옅은 맥박 */
.mg--vital .l3 {
  inset: 0;
  background: radial-gradient(circle at 50% 50%, rgba(255, 240, 220, 0.18), transparent 65%);
  animation: vt-pulse 1.4s ease-in-out infinite;
}

@keyframes vt-orbit {
  to {
    transform: rotate(360deg);
  }
}

@keyframes vt-orbit-rev {
  to {
    transform: rotate(-360deg);
  }
}

@keyframes vt-pulse {
  0%,
  100% {
    opacity: 0.35;
    transform: scale(1);
  }
  35% {
    opacity: 1;
    transform: scale(1.06);
  }
}

/* ============ 축제 — 엇박으로 점멸하는 보케 불빛 ============ */
.mg--festive {
  background: linear-gradient(160deg, #ff9db4 0%, #e56585 35%, #d94f70 60%, #6d2440 100%);
}

/* 광점 세트 A: 분홍 계열, 2.2초 점멸 */
.mg--festive .l1 {
  inset: 0;
  filter: blur(6px);
  background:
    radial-gradient(circle at 18% 24%, rgba(255, 214, 228, 0.95) 0 4.5%, transparent 7%),
    radial-gradient(circle at 62% 14%, rgba(255, 214, 228, 0.8) 0 3%, transparent 5.5%),
    radial-gradient(circle at 38% 68%, rgba(255, 214, 228, 0.85) 0 5.5%, transparent 8.5%),
    radial-gradient(circle at 86% 58%, rgba(255, 214, 228, 0.75) 0 3.5%, transparent 6%);
  animation: fs-blink 2.2s ease-in-out infinite alternate;
}

/* 광점 세트 B: 금색 계열, 3.1초 + 위상 오프셋 → 박자가 겹친다 */
.mg--festive .l2 {
  inset: 0;
  filter: blur(6px);
  background:
    radial-gradient(circle at 78% 26%, rgba(255, 227, 176, 0.9) 0 5%, transparent 8%),
    radial-gradient(circle at 30% 42%, rgba(255, 227, 176, 0.75) 0 3%, transparent 5.5%),
    radial-gradient(circle at 58% 84%, rgba(255, 227, 176, 0.85) 0 4.5%, transparent 7.5%),
    radial-gradient(circle at 10% 78%, rgba(255, 227, 176, 0.7) 0 3.5%, transparent 6%);
  animation: fs-blink 3.1s ease-in-out 0.8s infinite alternate;
}

/* 잘게 흩날리며 떠오르는 반짝임 */
.mg--festive .l3 {
  inset: 0;
  opacity: 0.3;
  background-image: radial-gradient(circle, rgba(255, 240, 246, 0.9) 0 1.5px, transparent 3px);
  background-size: 64px 88px;
  animation: fs-rise 7s linear infinite;
}

@keyframes fs-blink {
  from {
    opacity: 0.25;
  }
  to {
    opacity: 1;
  }
}

@keyframes fs-rise {
  to {
    background-position: 0 -88px;
  }
}

/* ============ 미식 — 피어오르는 김과 캔들 플리커 ============ */
.mg--gourmet {
  background: linear-gradient(165deg, #f7d3b8 0%, #dd9a63 40%, #c9713c 68%, #7a3f1c 100%);
}

/* 램프: 불규칙하게 일렁이는 따뜻한 빛 */
.mg--gourmet .l1 {
  width: 62%;
  height: 62%;
  left: -8%;
  top: -14%;
  border-radius: 50%;
  filter: blur(34px);
  background: radial-gradient(circle, rgba(255, 227, 186, 0.95), rgba(255, 227, 186, 0) 70%);
  animation: gm-flicker 3.2s ease-in-out infinite;
}

/* 김 두 줄기: 아래에서 피어올라 흔들리며 사라진다 */
.mg--gourmet .l2 {
  width: 12%;
  height: 46%;
  left: 34%;
  bottom: -6%;
  border-radius: 50%;
  filter: blur(13px);
  background: rgba(255, 241, 222, 0.55);
  animation: gm-steam 5.5s ease-in-out infinite;
}

.mg--gourmet .l3 {
  width: 9%;
  height: 40%;
  left: 58%;
  bottom: -6%;
  border-radius: 50%;
  filter: blur(12px);
  background: rgba(255, 241, 222, 0.45);
  animation: gm-steam 6.5s ease-in-out 2.6s infinite;
}

@keyframes gm-flicker {
  0%,
  100% {
    opacity: 0.75;
    transform: scale(1);
  }
  38% {
    opacity: 1;
    transform: scale(1.06);
  }
  52% {
    opacity: 0.82;
    transform: scale(0.99);
  }
  71% {
    opacity: 0.95;
    transform: scale(1.04);
  }
}

@keyframes gm-steam {
  0% {
    transform: translateY(28%) scaleY(0.7) rotate(0deg);
    opacity: 0;
  }
  35% {
    opacity: 0.8;
  }
  65% {
    transform: translateY(-52%) scaleY(1.1) rotate(5deg);
    opacity: 0.45;
  }
  100% {
    transform: translateY(-110%) scaleY(1.25) rotate(-4deg);
    opacity: 0;
  }
}

/* ============ 도심 산책 — 패럴랙스로 지나가는 쇼윈도 ============ */
.mg--citywalk {
  background: linear-gradient(150deg, #d9f2e6 0%, #8cc7ae 40%, #5b9e86 68%, #2f5c4a 100%);
}

/* 먼 블록: 옅고 느리게 (12초) — 가장자리가 또렷한 사각 띠 */
.mg--citywalk .l1 {
  width: 200%;
  height: 100%;
  left: 0;
  top: 0;
  background: repeating-linear-gradient(
    90deg,
    rgba(240, 252, 246, 0.28) 0 64px,
    transparent 64px 118px,
    rgba(240, 252, 246, 0.16) 118px 156px,
    transparent 156px 250px
  );
  animation: cw-pan 12s linear infinite;
}

/* 가까운 블록: 진하고 빠르게 (6.5초) — 시차가 걷는 원근을 만든다 */
.mg--citywalk .l2 {
  width: 200%;
  height: 100%;
  left: 0;
  top: 0;
  background: repeating-linear-gradient(
    90deg,
    transparent 0 210px,
    rgba(27, 61, 48, 0.3) 210px 300px,
    transparent 300px 480px
  );
  animation: cw-pan 6.5s linear infinite;
}

/* 걷는 리듬: 화면 전체가 1.3초 주기로 아주 살짝 위아래로 */
.mg--citywalk .l3 {
  inset: 0;
  background: linear-gradient(rgba(255, 255, 255, 0.07), transparent 30%);
  animation: cw-bob 1.3s ease-in-out infinite alternate;
}

@keyframes cw-pan {
  to {
    transform: translateX(-50%);
  }
}

@keyframes cw-bob {
  from {
    transform: translateY(-0.6%);
  }
  to {
    transform: translateY(0.6%);
  }
}

@media (prefers-reduced-motion: reduce) {
  .mg > span {
    animation: none;
  }
}
</style>