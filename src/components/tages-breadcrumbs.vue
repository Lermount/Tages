<template>
    <nav class="breadcrumbs">
        <a class="breadcrumbs-link" href="#">Главная</a>
        <span class="breadcrumbs-sep">/</span>
        <a class="breadcrumbs-link" href="#">Системы хранения</a>
        <span class="breadcrumbs-sep">/</span>
        <a class="breadcrumbs-link active-link" href="#">Комплекты стеллажных систем</a>
    </nav>
</template>
<script>
export default {

}
</script>
<style scoped>
.breadcrumbs {
    margin-bottom: 32px;
    display: flex;
    width: max-content;
    align-items: center;
}

.breadcrumbs-link,
.breadcrumbs-sep {
    font-family: SFUIText;
    font-size: 16px;
    font-weight: 400;
    line-height: 16px;
    letter-spacing: 0em;
    text-align: left;
    color: #727783;
    transition: .3s linear;
}

.breadcrumbs-sep {
    margin-left: 14px;
    margin-right: 14px;
}

.breadcrumbs-link:hover,
.active-link {
    color: #000;
    transition: .3s linear;
}

.breadcrumbs-link:active {
    opacity: 0.1;
}

@media(max-width: 710px) {

    .breadcrumbs-link,
    .breadcrumbs-sep {
        font-size: 9px;
    }

    .breadcrumbs-sep {
        margin-right: 2px;
        margin-left: 2px;
    }
    
}
</style>