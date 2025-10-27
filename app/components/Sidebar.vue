<script setup lang="ts">
</script>
<template>
        <aside class="[grid-area:aside] bg-blue-600">
            <div>icon</div>
            <div>select</div>
            <div>
                <ul>
                    <li>
                        <a href="#">
                            <UIcon name="i-lucide-lightbulb" class="size-5" />
                            <span>Home</span>
                        </a>
                    </li>
                    <li>
                        <a href="#">
                            <UIcon name="i-lucide-lightbulb" class="size-5" />
                            <span>Lessons</span>
                        </a>
                    </li>
                    <li>
                        <a href="#">
                            <UIcon name="i-lucide-lightbulb" class="size-5" />
                            <span>Video Exchanges</span>
                        </a>
                    </li>
                    <li>
                        <a href="#">
                            <UIcon name="i-lucide-lightbulb" class="size-5" />
                            <span>Study Materials</span>
                        </a>
                    </li>
                    <li>
                        <a href="#">
                            <UIcon name="i-lucide-lightbulb" class="size-5" />
                            <span>Bookmarks</span>
                        </a>
                    </li>
                    <li>
                        <a href="#">
                            <UIcon name="i-lucide-lightbulb" class="size-5" />
                            <span>History</span>
                        </a>
                    </li>
                </ul>
                <ul>
                    <li>
                        <a href="#">
                            <UIcon name="i-lucide-lightbulb" class="size-5" />
                            <span>Metronome</span>
                        </a>
                    </li>
                    <li>
                        <a href="#">
                            <UIcon name="i-lucide-lightbulb" class="size-5" />
                            <span>Scale Browser</span>
                        </a>
                    </li>
                </ul>
                <ul>
                    <li>
                        <a href="#">
                            <UIcon name="i-lucide-lightbulb" class="size-5" />
                            <span>Community</span>
                        </a>
                    </li>
                </ul>
            </div>
            <div>
                <ul>
                    <li>
                        <a href="#">
                            <UIcon name="i-lucide-lightbulb" class="size-5" />
                            <span>My Account</span>
                        </a>
                    </li>
                </ul>
            </div>
        </aside>
</template>
<style scoped>

aside {
    padding:20px;

    grid-template-columns: auto;
    grid-template-rows: auto auto 1fr auto;
}
aside ul {
    list-style-type:none;
    color:#FFF;
    margin:0;
    padding:0;
}
aside li {
    margin:.25rem 0;
}
aside a  {
    color:#FFF;
    width:100%;
    border-radius: .25rem;
    padding:.75rem;
    display: block
}
aside a:hover{
    background-color:rgba(255, 255, 255, .25)
}
</style>