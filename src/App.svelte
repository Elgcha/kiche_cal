<script lang="ts">
  let 연봉 = 0;
  let 근무시간 = 226;
  let 복리후생비 = 140000;
  let 야간 = 0;
  let 주말 = 0;
  let 휴가 = 0;
  let 결과 = {'시급': 0, '기본급': 0, '야간': 0, '주말': 0, '휴가': 0, '월급': 0, '소계': 0};
  let 미결과 = {'시급': 0, '기본급': 0, '야간': 0, '주말': 0, '휴가': 0, '월급': 0, '소계': 0};
  $: view = 1;
  let 결과2 = {'시급': 0, '기본급': 0, '야간': 0, '주말': 0, '휴가': 0, '월급': 0, '소계': 0};
  const check = () => {
    결과.월급 = ((연봉 + (복리후생비*12)) / 12)
    결과.시급 = (결과.월급 / (근무시간 + 야간*1.5 + 주말*2 + 휴가*1.5))
    결과.야간 = (결과.시급 * 야간 * 1.5)
    결과.주말 = (결과.시급 * 주말 * 2)
    결과.휴가 = (결과.시급 * 휴가 * 1.5)
    결과.소계 = 결과.월급 - 결과.야간 - 결과.주말 - 결과.휴가
    결과.기본급 = 결과.소계 - 복리후생비
    미결과.기본급 = 결과.기본급
    미결과.소계 = 미결과.기본급 + 복리후생비 - (9620*근무시간/100)
    미결과.시급 = (미결과.소계 / 226)
    return
  }
  const check2 = () => {
    결과2.월급 = ((연봉) / 12)
    결과2.시급 = (결과2.월급 / (근무시간 + 야간*1.5 + 주말*2 + 휴가*1.5))
    결과2.야간 = (결과2.시급 * 야간 * 1.5)
    결과2.주말 = (결과2.시급 * 주말 * 2)
    결과2.휴가 = (결과2.시급 * 휴가 * 1.5)
    결과2.소계 = 결과2.월급 - 결과2.야간 - 결과2.주말 - 결과2.휴가
    결과2.기본급 = 결과2.소계

    return
  }
</script>

<main>
  <button on:click={()=>{view=1;check()}}>복리후생비 포함</button>
  <button on:click={()=>{view=0;check2()}}>복리후생비 비포함</button>
  {#if view}
  <form on:submit|preventDefault={()=>{check()}}>
    <p>기본 근무시간</p>
    <input type="number" bind:value={근무시간} on:change={()=>check()}>
    <p>연봉</p>
    <input type="number" bind:value={연봉} on:change={()=>check()}>
    <p>복리후생비</p>
    <input type="number" bind:value={복리후생비} on:change={()=>check()}>
    <p>연장</p>
    <p>연장 150%</p>
    <input type="number" name="" id="" bind:value={야간} placeholder="시간" on:change={()=>check()}>
    <p>야간 200%</p>
    <input type="number" name="" id="" bind:value={주말} on:change={()=>check()}>
    <p>휴일 150%</p>
    <input type="number" name="" id="" bind:value={휴가} on:change={()=>check()}>
    <p>복리후생비는 반드시 있다고 가정, 9620 * {근무시간} = {9620*근무시간}의 1%인 {Math.round(9620*근무시간/100)}원 미산입으로 계산</p>
    <div>모든 계산 결과는 소수점 첫째 자리에서 반올림</div>
  </form>
  <hr>
  {#if 결과.기본급 != 0}
  <div>복리후생비 포함</div>
  <div>연봉: {Math.round(연봉 + 복리후생비*12)}</div>
  <div>월급: {Math.round(결과.월급)}</div>
  <div>시급: {Math.round(결과.시급)}</div> 
  <div>기본급: {Math.round(결과.기본급)}</div> 
  <div>소계(기본급+복리후생비): {Math.round(결과.소계)}</div> 
  <div>연장: {Math.round(결과.야간)}</div> 
  <div>야간: {Math.round(결과.주말)}</div> 
  <div>휴일: {Math.round(결과.휴가)}</div>
  <hr>
  {/if}
  {#if 미결과.기본급 != 0}
  <div>복리후생비 {Math.round(9620*근무시간/100)} 미산입 계산 </div>
<div>시급: {Math.round(미결과.시급)}</div> <div>기본급: {Math.round(미결과.기본급)}</div> <div>소계: {Math.round(미결과.소계)}</div>
  {/if}
{:else}
<form on:submit|preventDefault={()=>{check2()}}>
  <p>기본 근무시간</p>
  <input type="number" bind:value={근무시간} on:change={()=>check2()}>
  <p>연봉</p>
  <input type="number" bind:value={연봉} on:change={()=>check2()}>
  <p>복리후생비</p>
  <input type="number" bind:value={복리후생비} on:change={()=>check2()}>
  <p>연장</p>
  <p>연장 150%</p>
  <input type="number" name="" id="" bind:value={야간} placeholder="시간" on:change={()=>check2()}>
  <p>야간 200%</p>
  <input type="number" name="" id="" bind:value={주말} on:change={()=>check2()}>
  <p>휴일 150%</p>
  <input type="number" name="" id="" bind:value={휴가} on:change={()=>check2()}>
  <div>모든 계산 결과는 소수점 첫째 자리에서 반올림</div>
</form>
<hr>
{#if 결과2.시급 != 0}

<div>복리후생비: {Math.round(복리후생비*12)}</div>
<div>월급: {Math.round(결과2.월급)}</div>
<div>시급: {Math.round(결과2.시급)}</div> 
<div>기본급: {Math.round(결과2.기본급)}</div> 
<div>소계: {Math.round(결과2.소계)}</div> 
<div>연장: {Math.round(결과2.야간)}</div> 
<div>야간: {Math.round(결과2.주말)}</div> 
<div>휴일: {Math.round(결과2.휴가)}</div>
<hr>
{/if}

{/if}


</main>

<style>

</style>