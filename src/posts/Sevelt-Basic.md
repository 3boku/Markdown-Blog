---
title: Svelte Basic
description: 스벨트 기초 문법 정리.
date: '2024-1-23'
categories:
    - sveltekit
    - svelte
published: true
---

# svelte-basic
Svelte Learn!

## 상태값
```html
<script>
	let count = 0;
	//상태는 변수 뿐아니라 배열, object, object array등 js에 대부분에 타입을 지정할 수 있음
	function handleClick() {
		count += 1;
	}
</script>

<button on:click={handleClick}>
	클릭수 { count }
</button>
```

## 반응성
```html
<script>
	let count = 0
	$: doubled = count * 2

	$: if(count >= 10) {
		alert('카운트 10을 넘었습니다.')
		count = 9
	}
	//if문도 사용 가능
	$: {
		console.log( count )
		console.log( doubled )
	}
	//반응성은 $로 표시하고 함께 사용할 수 있음
	function handleClick() {
		count += 1;
	}
</script>

<button on:click={handleClick}>
	클릭수 { count } { count === 1 ? 'time' : 'times'}
</button>

<p>{count} 두배는 {doubled}</p>
```
상태값 변경을 동시적으로 빠르게 할 수 있음

## Component
```html
<script>
	import Header from './header.svelte'
	import Content from './content.svelte'
	import Footer from './footer.svelte'
</script>

<Header />
<Content />
<Content />
<Content />
<Footer/>
```
컴포넌트를 import해올때는 대문자 시작해야하고
html단에서 그냥 태그 쓰는것처럼 쓰면 불러올 수 있응

## props
```html
//app.svelte
<script>
	import CountComp from './countComp.svelte'
	import BtnComp from './btnComp.svelte'
	let count = 0
	$: doubled = count * 2

	$: if(count >= 10) {
		alert('카운트 10을 넘었습니다.')
		count = 9
	}

	function handleClick() {
		count += 1;
	}
</script>

<BtnComp { count } { handleClick }/>

<CountComp { count } { doubled }/>
```

```html
<script>
    //btnComp.svelte
	export let count;
	export let handleClick;

	import LabelComp from './labelComp.svelte'
</script>

<button on:click={handleClick}>
	클릭수 { count } <LabelComp {count}/>
</button>

```
props는 부모컴포넌트가 자식컴포넌트에게만 보낼 수 있음
함수던 변수던 `export let 변수명`하면 자식컴포넌트에서 사용 가능

`단방향 바인딩`임
그래서 context, dipatcher, store를 사용하면 컴포넌트끼리 통신할 수 있음

## 논리블록
```html
<script>
	let auth = {
		loggedIn: false
	}

	const handleLogin = () => auth.loggedIn = true;
	const handleLogout = () => auth.loggedIn = false;
	
</script>

<button on:click={handleLogin}>Log In </button>
<button on:click={handleLogout}>Log out</button>

{#if auth.loggedIn === true}
	<p>로그인 상태입니다.</p>
{:else}
 <p>로그아웃 상태입니다.</p>
{/if}    
```
저렇게 #if를 치면 조건문을 쓸 수 있음 마지막엔 /if로 닫아줘야함

## 반복블록
```html
<script>
	let todos = [
		{
			id: 0,
			content: '첫 번째 할일',
			done: false
		},
		{
			id: 1,
			content: '두 번째 할일',
			done: false
		},
		{
			id: 2,
			content: '세 번째 할일',
			done: false
		},
		{
			id: 3,
			content: '네 번째 할일',
			done: false
		}
	]
</script>

<ul>
	{#each todos as todo}
		<li>
			<span>
				{todo.id}
			</span>
			<span>
				{todo.content}
			</span>
			<span>
				{todo.done}
			</span>
		</li>
	{/each}
</ul>
```
배열에 내용을 #each를 사용해서 연속적으로 사용할 수 있음

## store
전역으로 사용가능한 상태값 저장소

```js
import { writable } from 'svelte/store';


function createCount() {
    const { subscribe, set, update } = writable(0);
    
    const increment = () => update(count => count +1)
    //update는 값을 받아와 연산하고, 값을 다시 리턴해줌
    const decrement = () => update(count => count -1)
    const reset = () => set(0)
    //set은 값을 초기화해줌
    return {
        subscribe,
        increment,
        decrement,
        return
    }
    //안에 내용은 무조건 리턴해줘야함

    export cosnt count = createCount();
}
//사용자 지정 함수 사용법은 변수처럼 똑같음
```
//writable을 import해온 후 저렇게 export로 선언하면 상태값 저장소가 생기고 저걸 컴포넌트에서 사용할때는 컴포넌트 import해오는거처럼 사용하면 됨

## slot
컴포넌트 재사용할때 사용함

```html
card.svelte

<article class="contact-card">
	<h2>
		<slot name="name">
			<span class="missing">미입력</span>
		</slot>
	</h2>

	<div class="address">
		<slot name="address">
			<span class="missing">미입력</span>
            //Missing을 쓰면 입력되지 않았을때 미입력이라고 출력가능함
		</slot>
	</div>

	{#if $$slots.email}
    //'$$slots.슬롯이름'을 적으면 슬롯이 사용되었는지 안되었는지 확인할 수 있음
		<div class="email">
			<hr>
			<slot name="email"></slot>
		</div>
	{/if}
</article>
```

```html
<script>
	import Card from './card.svelte'
</script>

<Card>
	<span slot="name">
		홍길동
	</span>

	<span slot="address">
		tjdnfxmrquftl<br>
		여의도동
	</span>					

</Card>
<Card>
	<span slot="name">
		홍길동
	</span>
    //slot클래스마다 다르게 넣어서 이름과 태그를 바꿔쓸수 있음

	<span slot="address">
		tjdnfxmrquftl<br>
		여의도동
	</span>					

	<span slot="email">
		a243qw@masdgas.com
	</span>	
</Card>
```

div말고 svelte:fragment를 사용하면 div대용으로 사용가능