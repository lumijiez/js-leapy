<script>
    import App from '$lib/components/valentines/ValentineApp.svelte'
  </script>
  
  <div>
    <App />
  </div>
  
  <style>
    :global(body) {
      margin: 0;
      overflow-y: hidden;
      overflow-x:hidden;
    }
  
    div {
      width: 100vw;
      height: 100vh;
      background: #DAA0DC;
      background: linear-gradient(180deg, rgba(13, 19, 32, 1) 0%, rgba(8, 12, 21, 1) 100%);
    }
  </style>
  