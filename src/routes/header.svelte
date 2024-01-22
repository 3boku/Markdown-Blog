<script lang="ts">
    import * as config from "$lib/config"
    import Toggle from './togle.svelte'
</script>

<nav>
    <a href="/" class="title">
        <b>Sambok's svelte blog</b>
    </a>

    <ul class="links">
        <li>
            <a href="https://3boku.me">About</a>
        </li>
        <li>
            <a href="https://linktr.ee/3boku">Contact</a>
        </li>
        <li>
            <a href="/post">Post</a>
        </li>
    </ul>

    <Toggle />
</nav>

<style>
    nav {
        padding-block: var(--size-7);
    }

    .links {
        margin-block: var(--size-7);
    }

    a {
        color: inherit;
        text-decoration: none;
    }

    @media (min-width: 768px) {
        nav {
            display: flex;
            justify-content: space-between;
        }

        .links {
            display: flex;
            gap: var(--size-7);
            margin-block: 0;
        }
    }
</style>