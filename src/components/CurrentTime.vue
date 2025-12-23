<template>
    <div class="cardBox">
        <div class="container">
            <h2>Сегодня :</h2>
            <h3>{{ currentTime }}</h3>
        </div>
    </div>
</template>

<script>
    // export default {
    //     name: 'CurrentTime',
    //     computed: {
    //         getCurrentDate() {
    //             const  browserLocale = 
    //                 navigator.languages && navigator.languages.length
    //                 ? navigator.languages[0]
    //                 : navigator.language;
    //             const intDateTime = new Intl.DateTimeFormat(
    //                 browserLocale, {
    //                     year: 'numeric',
    //                     month: 'numeric',
    //                     day: 'numeric',
    //                     hour: 'numeric',
    //                     minute: 'numeric'
    //                 }
    //             );
    //             return intDateTime.format(new Date());
    //         }
    //     }
    // };

import { ref, onMounted, onUnmounted } from 'vue';

export default {
  setup() {
    const currentTime = ref('');

    // функция для обновления времени
    const updateTime = () => {
      const now = new Date();
      currentTime.value = now.toLocaleString(); // или форматируйте по желанию
    };

    let timer = null;

    onMounted(() => {
      updateTime(); // установить время сразу при монтировании
      timer = setInterval(updateTime, 1000); // обновлять каждую секунду
    });

    onUnmounted(() => {
      clearInterval(timer); // очищаем таймер при размонтировании
    });

    return {
      currentTime,
    };
  },
};

</script>