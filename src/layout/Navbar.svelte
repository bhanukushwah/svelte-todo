<script>
    import { Link } from 'svelte-routing';

    document.addEventListener('DOMContentLoaded', function() {
    var elems = document.querySelectorAll('.sidenav');
    var instances = M.Sidenav.init(elems);
  });
</script>

<div>
  <nav>
    <div class="nav-wrapper container">
      <Link to="/" ><span class="brand-logo">Logo</span></Link>
      <ul class="right hide-on-med-and-down">
        <li><Link to="/">Home</Link></li>
        <li><Link to="/about">About</Link></li>
    </div>
  </nav>
</div>