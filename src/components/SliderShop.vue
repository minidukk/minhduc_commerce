<template>
<div id="carouselExampleIndicators" class="carousel slide" data-bs-ride="carousel" height="200">
  <div class="carousel-inner">
    <div class="carousel-item active item">
      <img src="https://i0.wp.com/picjumbo.com/wp-content/uploads/beautiful-nature-mountain-scenery-with-flowers-free-photo.jpg?w=600&quality=80" class="d-block w-100" alt="...">
    </div>
    <div class="carousel-item item">
      <img src="https://media.istockphoto.com/id/1093110112/photo/picturesque-morning-in-plitvice-national-park-colorful-spring-scene-of-green-forest-with-pure.jpg?s=612x612&w=0&k=20&c=lpQ1sQI49bYbTp9WQ_EfVltAqSP1DXg0Ia7APTjjxz4=" class="d-block w-100" alt="...">
    </div>
    <div class="carousel-item item">
      <img src="https://img.freepik.com/free-photo/wide-angle-shot-single-tree-growing-clouded-sky-during-sunset-surrounded-by-grass_181624-22807.jpg?size=626&ext=jpg&ga=GA1.1.1826414947.1699228800&semt=sph" class="d-block w-100" alt="...">
    </div>
  </div>
  <button class="carousel-control-prev" type="button" data-bs-target="#carouselExampleIndicators" data-bs-slide="prev">
    <span class="carousel-control-prev-icon" aria-hidden="true"></span>
    <span class="visually-hidden">Previous</span>
  </button>
  <button class="carousel-control-next" type="button" data-bs-target="#carouselExampleIndicators" data-bs-slide="next">
    <span class="carousel-control-next-icon" aria-hidden="true"></span>
    <span class="visually-hidden">Next</span>
  </button>
</div>
</template>

<style>
.carousel .item {
  height: 500px;
}

.item img {
    position: absolute;
    top: 0;
    left: 0;
    object-fit: cover;
    height: 500px;
}
</style>