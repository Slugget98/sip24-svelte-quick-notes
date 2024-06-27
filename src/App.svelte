<script>

import { onMount } from "svelte";

  let pages = [];
  let currentPageIndex = 0;
  let title = '';
  let note = '';

  onMount(() => {
    const savedPages = localStorage.getItem("pages");
    if(savedPages){
      pages=JSON.parse(savedPages)
      title = pages[currentPageIndex];
      note = localStorage.getItem(title);
    }else{
      addPage();
    }
  });

  function saveFun(){
    const storedPageName = pages[currentPageIndex]
    if (storedPageName != title){
      localStorage.removeItem(storedPageName);
      pages[currentPageIndex] = title;
    }
    localStorage.setItem(title,note);
    localStorage.setItem("pages",JSON.stringify(pages));
  }

  function addPage(){
    pages.push("New Page");
    selectPage(pages.length ? pages.length-1 : 0);
  }

  function selectPage(index){
    currentPageIndex=index;
    title = pages[currentPageIndex];
    note = localStorage.getitem(title)


  }

</script>

<aside class="fixed top-0 left-0 z-49 w-60 h-screen">
<div class="bg-gray-50 overflow-y-auto py-5 px-3 h-full border-r border-gray-200">
  <ul>
    {#each pages as page, index}
    <li>
      <button on:click={()=> selectPage(index)} class="{index == currentPageIndex ? 'bg-gray-300' : ''} py-2 px-3 text-gray-900 rounded-lg">{page}</button>
    </li>
    {/each}
    <li class=text-center>
    <button on:click={addPage} class="font-medium">+ Add Page</button>
  </li>
  </ul>
</div>
</aside>

<main class="p-4 ml-60 h-auto">
  <div class="grid grid-cols-2 items-center mb-3">
    <h1 class="text-3xl font-bold" contenteditable bind:textContent={title}></h1>
    <button class="ml-auto rounded-lg bg-blue-500 py-2 px-4 m-1 text-white hover:bg-blue-600" on:click={saveFun}>Save</button>
  </div>
<hr>
  <textarea class="block w-full rounded-lg text-gray-800 bg-gray-100" bind:value={note}></textarea>
</main>

<style>
  
</style>
