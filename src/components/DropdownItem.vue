<template>
    <router-link :to="{ name: linkName}" class="menu-item">
        <span :class="'icon-button' + ' ' + iconClass ">{{icon}}</span>
        {{text}}
    </router-link>
</template>

<script>
export default {
    name: 'DropdownItem',
    props: {
        link: {
            type: Object,
            default: () => {} 
        },
        text: {
            type: String,
            default: ""
        },
        iconClass: {
            type: String,
            default: "material-icons"
        },
        icon: {
            type: String,
            default: "disabled_by_default"
        },
        linkName: {
            type: String,
            default: "homepage"
        }
    },
}
</script>

<style>
.menu-item {
    height: 50px;
    display: flex;
    align-items: center;
    border-radius: 8px;
    transition: background 500ms;
    padding: 0.5rem;
    color: var(--black);
}

.menu-item .icon-button {
    margin-right: 0.5rem;
}

.menu-item .icon-button:hover {
    filter: none;
}

.menu-item:hover {
    background-color: rgb(236, 236, 236);
    color: var(--orange-primary);

}

.icon-right {
    margin-left: auto;
}
</style>