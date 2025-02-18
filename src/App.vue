<script setup>
import { computed, onMounted, provide, reactive, ref, watch } from 'vue';
import axios from 'axios';


const trade = reactive({
    RUB: null,
    USDT: null,
})


const USDTPrice = ref(0)
const buy = ref(true)
const reloadPrice = ref(false)
const errPrice = ref(false)

const tradeRubUSDT = () => {
    trade.RUB = parseInt((trade.USDT * USDTPrice.value) * 100) / 100
}

const tradeUSDTRub = () => {
    trade.USDT = parseInt((trade.RUB / USDTPrice.value) * 100) / 100
}

const changeReload = () => {
    reloadPrice.value = false
}

const onReloadPrice = () => {
    if(!reloadPrice.value) {
        reloadPrice.value = true
        setTimeout(changeReload, 2000)
    }
}

const fetchPrice = async () => {
    try {
        if(!reloadPrice.value) {
            const { data } = await axios.get('https://min-api.cryptocompare.com/data/price?fsym=USDT&tsyms=RUB')
            USDTPrice.value = data.RUB
        }
    } catch(e) {
        errPrice.value = true
    }
}

onMounted( async () => {
    await fetchPrice()
})

watch(USDTPrice, () => {
    if(buy.value === true) {
        trade.USDT = trade.RUB / USDTPrice.value === 0 ? null : trade.USDT = parseInt((trade.RUB / USDTPrice.value) * 100) / 100
    } else {
        trade.RUB = trade.USDT * USDTPrice.value === 0 ? null : trade.RUB = parseInt((trade.USDT * USDTPrice.value) * 100) / 100
    }
})

provide('buyChecked', {buy})

</script>

<template>
    <header class=" px-[60px] max-[768px]:px-[30px] max-[450px]:px-[15px] max-w-[1440px] m-auto">
        <div class="flex justify-between items-center py-2.5">
            <router-link to="/">
                <h1 class=" text-[32px] max-[768px]:text-[28px] max-[450px]:text-[22px] font-bold text-[#009000] tracking-widest uppercase">Yar Exchange</h1>
            </router-link>

            <nav class="flex gap-10 max-[1170px]:hidden">
                <a href="#calculator" class=" hover:opacity-70 transition" @click="buy = true">Купить криптовалюту</a>
                <a href="#calculator" class=" hover:opacity-70 transition" @click="buy = false">Продать криптовалюту</a>
            </nav>

            <div class="flex items-center gap-10">
                <a href="tel:89010447605" class=" hover:opacity-70 transition max-[650px]:hidden">+7 (901) 044-76-05</a>

                <a href="https://t.me/YARExchange" target="_blank" class="flex items-center gap-2 hover:opacity-70 transition">
                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 30 31" class="w-[40px] h-[40px] max-[1170px]:w-[30px] max-[1170px]:h-[30px]">
                        <g clip-path="url(#telegram_svg__a)">
                            <path fill="#009000" d="M14.991.964c-8.284 0-15 6.715-15 15s6.716 15 15 15c8.285 0 15-6.716 15-15 0-8.285-6.715-15-15-15m0 .882c7.798 0 14.118 6.32 14.118 14.118 0 7.797-6.32 14.117-14.118 14.117-7.797 0-14.117-6.32-14.117-14.117S7.194 1.845 14.99 1.845m6.165 8.212c-.347-.04-.78.053-1.267.229-.889.32-12.245 5.143-12.901 5.421-.622.265-1.21.553-1.212.97 0 .294.175.459.655.63.5.178 1.758.56 2.5.765.717.197 1.532.025 1.988-.259.483-.3 6.069-4.037 6.47-4.365.4-.327.718.093.39.42-.328.329-4.16 4.049-4.666 4.564-.614.626-.178 1.274.234 1.534.47.296 3.852 2.563 4.362 2.928.51.363 1.026.529 1.5.529.472 0 .72-.623.956-1.346.276-.846 1.57-9.284 1.728-10.947.048-.503-.11-.838-.422-.987a1 1 0 0 0-.315-.086"></path>
                        </g><defs><clipPath id="telegram_svg__a"><path fill="#fff" d="M-.009.964h30v30h-30z"></path></clipPath></defs>
                    </svg>
                    <p class="max-[370px]:hidden">Telegram</p>
                </a>
            </div>
        </div>
    </header>

    <main class="max-w-[1440px] m-auto">
        <section id="calculator" 
        class="mt-[100px] max-[768px]:mt-[30px] pt-[50px] flex px-[60px] max-[768px]:px-[30px] max-[450px]:px-[15px] gap-12
            max-[1170px]:flex-col max-[1170px]:items-center max-[1170px]:gap-[100px] max-[1170px]:content-center"
        >
            <div class=" max-w-[680px] max-[1170px]:w-[90%] max-[1170px]:text-center">
                <h1 class="text-[50px] max-[768px]:text-[24px] font-bold uppercase text-balance leading-[70px] max-[768px]:leading-[136%]">Обмен криптовалюты в <span class="text-[#009000]">России, Турции, ОАЭ, Китае, Америке</span> вместе с</h1>
                <p class="text-[50px] max-[768px]:text-[24px] uppercase tracking-widest text-[#009000] font-bold">YAR EXCHANGE</p>
                <h3 class="text-[20px] mt-3 max-[768px]:text-[14px]">Безопасный и быстрый обмен  <span class="text-[#009000]">криптовалют</span> на наличные.</h3>
                <h3 class="text-[20px] max-[768px]:text-[14px]">Наш офис находится в г. <span class="text-[#009000]">Ярославль</span>.</h3>

                <button class="bg-[#009000] py-3 max-[768px]:p-[10px] max-[768px]:w-full  rounded-md w-[400px] mt-[100px] max-[1170px]:mt-[50px] hover:opacity-85 duration-200">
                    <a href="https://t.me/YARExchange" target="_blank" class="flex items-center gap-5 justify-center text-[24px] max-[768px]:text-[18px]">
                        Написать нам 
                        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 30 31" class="w-[40px] h-[40px] max-[1170px]:w-[30px] max-[1170px]:h-[30px] max-[768px]:w-[25px] max-[768px]:h-[25px] ">
                            <g clip-path="url(#telegram_svg__a)">
                                <path fill="#ffffff" d="M14.991.964c-8.284 0-15 6.715-15 15s6.716 15 15 15c8.285 0 15-6.716 15-15 0-8.285-6.715-15-15-15m0 .882c7.798 0 14.118 6.32 14.118 14.118 0 7.797-6.32 14.117-14.118 14.117-7.797 0-14.117-6.32-14.117-14.117S7.194 1.845 14.99 1.845m6.165 8.212c-.347-.04-.78.053-1.267.229-.889.32-12.245 5.143-12.901 5.421-.622.265-1.21.553-1.212.97 0 .294.175.459.655.63.5.178 1.758.56 2.5.765.717.197 1.532.025 1.988-.259.483-.3 6.069-4.037 6.47-4.365.4-.327.718.093.39.42-.328.329-4.16 4.049-4.666 4.564-.614.626-.178 1.274.234 1.534.47.296 3.852 2.563 4.362 2.928.51.363 1.026.529 1.5.529.472 0 .72-.623.956-1.346.276-.846 1.57-9.284 1.728-10.947.048-.503-.11-.838-.422-.987a1 1 0 0 0-.315-.086"></path>
                            </g><defs><clipPath id="telegram_svg__a"><path fill="#fff" d="M-.009.964h30v30h-30z"></path></clipPath></defs>
                        </svg>
                    </a>
                </button>
            </div>

            <div class="min-w-[500px] max-[550px]:min-w-[400px] max-[470px]:min-w-[300px] max-[320px]:min-w-[280px] p-4 border border-[#009000] rounded-md bg-black bg-opacity-0 backdrop-blur-sm max-h-[400px]">
                <div class="flex items-start gap-2">
                    <div class=" border-b-[4px] py-3 flex justify-center w-1/2 text-[18px] opacity-50 hover:opacity-90 duration-300 cursor-pointer"
                        :class="buy ? 'border-b-[#009000] opacity-90' : ''"
                        @click="buy = true"
                    >
                        Купить
                    </div>
                    <div class="border-b-[4px] py-3 flex justify-center w-1/2 text-[18px] opacity-50 hover:opacity-90 duration-300 cursor-pointer"
                        :class="!buy ? 'border-b-[#009000] opacity-90' : ''"
                        @click="buy = false"
                    >
                        Продать
                    </div>
                </div>

                <div class=" px-3 flex flex-col gap-10 mt-[30px]"
                    :class="!buy ? 'flex-col-reverse' : ''"
                >
                    <div class=" border-b border-b-slate-100 ">
                        <div class="relative">
                            <input type="number" 
                                :placeholder="buy ? 'Отдаете' : 'Получаете'"
                                class="w-full appearance-none bg-transparent p-3 border-none focus:outline-none text-[18px]"
                                v-model="trade.RUB"
                                @input="tradeUSDTRub"
                            >

                            <p class="absolute right-0 top-2.5 flex items-center gap-2">
                                <span>RUB</span>
                                <img src="./assets/img/CASHRUB.svg" alt="" class="w-[28px]">
                            </p>
                        </div>
                    </div>

                    <div class=" border-b border-b-slate-100 ">
                        <div class="relative">
                            <input type="number" 
                                :placeholder="!buy ? 'Отдаете' : 'Получаете'"
                                class="w-full appearance-none bg-transparent p-3 border-none focus:outline-none text-[18px]"
                                v-model="trade.USDT"
                                @input="tradeRubUSDT"
                            >

                            <p class="absolute right-0 top-2.5 flex items-center gap-2">
                                <span>USDT</span>
                                <img src="./assets/img/USDT.svg" alt="" class="w-[28px]">
                            </p>
                        </div>
                    </div>
                </div>

                <div class="mt-5 px-3">
                    <div class="flex justify-between text-[14px] max-[360px]:text-[12px]">
                        <p>Текущий курс</p>

                        <div class="flex gap-2 items-center">
                            1 USDT ≈ {{ USDTPrice }} RUB

                            <svg data-tooltip-target="tooltip-default" version="1.1" @click="fetchPrice(), onReloadPrice()" class="w-[16px] cursor-pointer outline-none" :class="reloadPrice ? 'anim-reload' : ''" 
                            xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px" viewBox="0 0 512 512" style="enable-background:new 0 0 512 512;" xml:space="preserve">
                                <path style="fill:#71C285;" d="M472.615,295.385C472.615,415.015,375.631,512,256,512S39.385,415.015,39.385,295.385h59.077 c0,87.01,70.528,157.538,157.538,157.538s157.538-70.528,157.538-157.538S343.01,137.846,256,137.846v78.769L147.692,108.308 l0.985-0.985L256,0l0,0v78.769C375.631,78.769,472.615,175.754,472.615,295.385z"/>
                            </svg>

                            <div id="tooltip-default" role="tooltip" class=" absolute z-10 invisible inline-block px-1 py-1 text-[12px] font-medium transition-opacity duration-300 bg-gray-500 rounded-lg shadow-xs opacity-0 tooltip">
                                Обновить курс
                            </div>
                        </div>
                    </div>

                    <p v-if="errPrice" class="text-[12px] text-red-500 mt-2">Ошибка отображения курса, попробуйте еще раз</p>


                    <p class="text-[12px] text-slate-400 mt-3">* Размер комиссии за обмен обговаривается индивидуально. <br>
                        Для уточнения свяжитесь с  <span class="text-[#009000] cursor-pointer hover:border-b hover:border-b-[#009000]">нами</span>.
                    </p>
                </div>

            </div>
        </section>

        <section class=" bg-[#373737] px-[60px] max-[768px]:px-[30px] max-[450px]:px-[15px] py-[50px] mt-[120px] max-[768px]:mt-[80px] w-full bg-opacity-50">
            <h1 class=" text-[40px] max-[768px]:text-[30px] flex justify-center uppercase font-bold">Наши услуги</h1>

            <ul class="text-[24px] grid grid-cols-2 max-[1170px]:grid-cols-1 gap-x-[80px] gap-y-[130px] max-[768px]:gap-y-[80px] mt-20">
                <li class="flex gap-6 text-[30px] max-[768px]:text-[24px] items-center text-balance justify-self-center">
                    <p class="text-[50px] max-[768px]:text-[28px] font-bold text-[#009000] ">01.</p>
                    Обмен валют с наличной выдачей <br>или переводом на юр. лица.
                </li>

                <li class="flex gap-6 items-center text-balance justify-self-center text-[30px] max-[768px]:text-[24px]">
                    <p class="text-[50px] max-[768px]:text-[28px] font-bold text-[#009000]">02.</p>
                    Помощь в оплате за границей
                </li>

                <li class="flex gap-6 items-center text-balance justify-self-center text-[30px] max-[768px]:text-[24px]">
                    <p class="text-[50px] max-[768px]:text-[28px] font-bold text-[#009000]">03.</p>
                    Аренда карт иностранных банков
                </li>

                <li class="flex gap-6 items-center text-balance justify-self-center text-[30px] max-[768px]:text-[24px]">
                    <p class="text-[50px] max-[768px]:text-[28px] font-bold text-[#009000]">04.</p>
                    Оплата переводом за границу
                </li>
            </ul>

        </section>

        <section class="mt-[100px] px-[60px] max-[768px]:px-[30px] max-[450px]:px-[15px]">
            <h1 class="flex justify-center text-[40px] max-[768px]:text-[30px] max-[530px]:text-[26px] font-bold uppercase">Почему мы?</h1>

            <div class="grid grid-cols-4 max-[1080px]:grid-cols-2 max-[530px]:grid-cols-1 gap-10 mt-[80px]">
                <div class="bg-black bg-opacity-0 backdrop-blur-sm p-3 max-[768px]:p-2 rounded-lg border border-[#009000] flex justify-center">
                    <p class="flex items-center gap-3">
                        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 30 31" class="shrink-0 md:w-[28px] w-6 md:h-[28px] h-6"><path fill="#fff" fill-rule="evenodd" d="M12.25 12.19a2.75 2.75 0 1 1 5.5 0 2.75 2.75 0 0 1-5.5 0M15 10.94a1.25 1.25 0 1 0 0 2.5 1.25 1.25 0 0 0 0-2.5M15 15.44c-1.196 0-2.315.24-3.164.664-.803.402-1.586 1.096-1.586 2.085v.063c-.002.51-.004 1.37.81 1.96.378.273.877.447 1.495.558.623.113 1.422.17 2.445.17s1.822-.058 2.445-.17c.618-.11 1.117-.285 1.495-.559.814-.59.812-1.449.81-1.96v-.062c0-.99-.783-1.683-1.586-2.085-.849-.424-1.968-.665-3.164-.665m-3.25 2.75c0-.116.113-.422.757-.744.6-.3 1.48-.507 2.493-.507s1.894.207 2.493.507c.644.322.757.628.757.743 0 .605-.038.698-.19.807-.122.088-.373.207-.88.298-.502.09-1.203.145-2.18.145s-1.678-.055-2.18-.145c-.507-.091-.758-.21-.88-.298-.151-.11-.19-.202-.19-.807" clip-rule="evenodd"></path><path fill="#fff" fill-rule="evenodd" d="M11.723 5.24c1.444-.494 2.34-.8 3.277-.8s1.833.306 3.277.8l.727.25c1.481.507 2.625.898 3.443 1.23.412.168.767.33 1.052.496.275.16.55.358.737.625.185.263.281.588.341.9.063.324.1.713.125 1.16.048.887.048 2.102.048 3.678v1.602c0 6.1-4.608 9.026-7.348 10.223l-.027.012c-.34.148-.66.288-1.027.382-.387.099-.799.141-1.348.141-.55 0-.96-.042-1.348-.141-.367-.094-.687-.234-1.027-.382l-.027-.012C9.858 24.207 5.25 21.282 5.25 15.18v-1.602c0-1.576 0-2.791.048-3.678.025-.447.062-.836.125-1.16.06-.312.156-.637.34-.9.188-.267.463-.466.738-.625.285-.166.64-.328 1.052-.496.818-.332 1.962-.723 3.443-1.23zm3.277.7c-.658 0-1.305.211-2.92.764l-.572.196c-1.513.517-2.616.895-3.39 1.21a7 7 0 0 0-.864.403 2 2 0 0 0-.208.14.4.4 0 0 0-.055.05.4.4 0 0 0-.032.073q-.03.082-.063.248a7 7 0 0 0-.1.958c-.046.841-.046 2.016-.046 3.624v1.575c0 5.175 3.87 7.722 6.449 8.848.371.163.586.254.825.316.228.058.506.094.976.094s.748-.036.976-.094c.24-.062.454-.153.825-.316 2.58-1.126 6.449-3.673 6.449-8.848v-1.575c0-1.608 0-2.783-.046-3.624a7 7 0 0 0-.1-.958 2 2 0 0 0-.063-.248.4.4 0 0 0-.032-.073.4.4 0 0 0-.055-.05 2 2 0 0 0-.208-.14 7 7 0 0 0-.864-.403c-.774-.315-1.877-.693-3.39-1.21l-.573-.196c-1.614-.553-2.261-.765-2.919-.765" clip-rule="evenodd"></path></svg>
                        Конфиденциальность
                    </p>
                </div>

                <div class="bg-black bg-opacity-0 backdrop-blur-sm p-3 max-[768px]:p-2 rounded-lg border border-[#009000] flex justify-center">
                    <p class="flex items-center gap-3">
                        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 25 25" class="shrink-0 md:w-[28px] w-6 md:h-[28px] h-6"><path fill="#fff" fill-rule="evenodd" d="M14.565 6.139h-3.72a3 3 0 0 1-2.34 2.34v.52a3 3 0 0 1 2.34 2.34h3.72a3 3 0 0 1 2.34-2.34v-.52a3 3 0 0 1-2.34-2.34m.483-1.198q-.33-.004-.702-.003h-3.283q-.372 0-.702.003a.6.6 0 0 0-.13.002 9 9 0 0 0-.962.06c-.502.067-.956.215-1.32.58-.364.364-.513.818-.58 1.32-.038.278-.053.6-.06.961a.6.6 0 0 0-.002.13q-.003.33-.002.703v.083q0 .372.002.702a.6.6 0 0 0 .002.13c.007.363.022.684.06.962.067.502.216.956.58 1.32s.818.513 1.32.58c.278.038.6.053.961.06a.6.6 0 0 0 .131.002q.33.004.702.003h3.283q.373 0 .702-.003a.6.6 0 0 0 .13-.002c.363-.007.684-.022.962-.06.502-.067.956-.216 1.32-.58s.513-.818.58-1.32c.038-.278.054-.6.06-.961a.6.6 0 0 0 .002-.13q.004-.33.003-.703v-.083q0-.372-.003-.702a.6.6 0 0 0-.002-.13 9 9 0 0 0-.06-.962c-.067-.502-.215-.956-.58-1.32-.364-.365-.818-.513-1.32-.58a9 9 0 0 0-.961-.06.6.6 0 0 0-.13-.002m.772 1.233c.182.483.566.867 1.05 1.05l-.019-.161c-.05-.37-.135-.527-.24-.632s-.262-.19-.63-.24zm1.05 4.08a1.8 1.8 0 0 0-1.05 1.05q.084-.009.16-.019c.37-.05.527-.135.632-.24s.19-.262.24-.63zm-7.28 1.05a1.8 1.8 0 0 0-1.05-1.05q.008.084.018.16c.05.37.135.527.24.632s.262.19.631.24zm-1.05-4.08a1.8 1.8 0 0 0 1.05-1.05q-.085.007-.161.018c-.37.05-.527.135-.631.24-.105.104-.19.262-.24.63zm4.165 1.314a.2.2 0 1 0 0 .4.2.2 0 0 0 0-.4m-1.4.2a1.4 1.4 0 1 1 2.8 0 1.4 1.4 0 0 1-2.8 0m-1.253 5.959c1.492-.29 3.09-.224 4.383.547.182.108.352.243.5.41.301.338.457.758.464 1.178q.229-.148.462-.326l1.446-1.092a2.11 2.11 0 0 1 2.499 0c.669.505.935 1.411.456 2.179-.34.545-.852 1.3-1.374 1.782-.527.489-1.277.9-1.845 1.173-.689.333-1.433.517-2.157.634-1.48.24-3.019.203-4.481-.099a11.4 11.4 0 0 0-2.292-.234H6.305a.6.6 0 1 1 0-1.2h1.808c.85 0 1.707.089 2.534.259a11.3 11.3 0 0 0 4.047.09c.663-.108 1.283-.268 1.828-.53.547-.265 1.161-.612 1.55-.973.395-.365.836-.998 1.172-1.538.102-.162.088-.397-.161-.585a.91.91 0 0 0-1.053 0l-1.446 1.092c-.577.436-1.287.902-2.168 1.043a7 7 0 0 1-.42.054c-.45.045-.972.055-1.546 0a.6.6 0 1 1 .113-1.194c.501.048.952.037 1.334-.002l.023-.002a.594.594 0 0 0 .12-.91 1 1 0 0 0-.22-.178c-.944-.564-2.206-.658-3.54-.4a9.7 9.7 0 0 0-3.628 1.547.6.6 0 1 1-.694-.979 10.9 10.9 0 0 1 4.094-1.746" clip-rule="evenodd"></path></svg>
                        Выгодный курс
                    </p>
                </div>

                <div class="bg-black bg-opacity-0 backdrop-blur-sm p-3 max-[768px]:p-2 rounded-lg border border-[#009000] flex justify-center">
                    <p class="flex items-center gap-3">
                        <svg version="1.1" fill="white" xmlns="http://www.w3.org/2000/svg" class="shrink-0 w-5 h-5" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px" viewBox="0 0 512 512" xml:space="preserve"><path d="M176,472c5.52,0,10-4.48,10-10s-4.48-10-10-10s-10,4.48-10,10S170.48,472,176,472z"/><path d="M66,482c0,16.542,13.458,30,30,30h320c16.542,0,30-13.458,30-30V222c0-16.542-13.458-30-30-30h-10v-42 C406,67.29,338.71,0,256,0S106,67.29,106,150v42H96c-16.542,0-30,13.458-30,30V482z M126,372H86v-40h40V372z M86,392h40v40H86V392z M126,312H86v-40h40V312z M386,332h40v40h-40V332z M426,312h-40v-40h40V312z M386,392h40v40h-40V392z M126,150 c0-71.682,58.318-130,130-130s130,58.318,130,130v42h-40v-42c0-49.626-40.374-90-90-90s-90,40.374-90,90v42h-40V150z M186,192v-42 c0-38.598,31.402-70,70-70s70,31.402,70,70v42H186z M96,212h320c5.514,0,10,4.486,10,10v30h-50c-5.523,0-10,4.477-10,10v180 c0,5.523,4.477,10,10,10h50v30c0,5.514-4.486,10-10,10H96c-5.514,0-10-4.486-10-10v-30h50c5.523,0,10-4.477,10-10V262 c0-5.523-4.477-10-10-10H86v-30C86,216.486,90.486,212,96,212z"/><path d="M215.99,471H336c5.523,0,10-4.477,10-10s-4.477-10-10-10H215.99c-5.523,0-10,4.477-10,10S210.467,471,215.99,471z"/><path d="M256,371.29c-4.272,0-8.884-2.687-12.986-7.565c-3.554-4.227-9.862-4.774-14.089-1.219 c-4.228,3.554-4.773,9.862-1.219,14.089c5.346,6.359,11.632,10.79,18.294,13.024v11.671c0,5.523,4.477,10,10,10s10-4.477,10-10 v-11.72c11.639-4.128,20-15.243,20-28.28c0-16.542-13.458-30-30-30c-5.514,0-10-4.486-10-10s4.486-10,10-10 c3.542,0,7.283,1.807,10.816,5.227c3.97,3.841,10.3,3.737,14.14-0.233c3.84-3.969,3.736-10.3-0.233-14.14 c-5.075-4.911-10.153-7.688-14.724-9.205V281.29c0-5.523-4.477-10-10-10s-10,4.477-10,10v11.72c-11.639,4.128-20,15.243-20,28.28 c0,16.542,13.458,30,30,30c5.514,0,10,4.486,10,10S261.514,371.29,256,371.29z"/></svg>
                        Безопасные сделки
                    </p>
                </div>

                <div class="bg-black bg-opacity-0 backdrop-blur-sm p-3 max-[768px]:p-2 rounded-lg border border-[#009000] flex justify-center">
                    <p class="flex items-center gap-3">
                        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 25 25" class="shrink-0 md:w-[28px] w-6 md:h-[28px] h-6"><path fill="#fff" fill-rule="evenodd" d="M12.833 3.945h.083c.719 0 1.318 0 1.794.064.502.068.956.216 1.32.58.364.365.513.818.58 1.32.048.352.06.772.063 1.257.52.016.982.047 1.393.102.938.126 1.697.392 2.295.99.6.599.865 1.358.99 2.296.123.912.123 2.076.123 3.546v.09c0 1.47 0 2.635-.122 3.547-.126.938-.392 1.697-.99 2.295-.6.599-1.358.865-2.296.99-.912.123-2.076.123-3.546.123h-3.29c-1.47 0-2.635 0-3.547-.122-.938-.126-1.697-.392-2.296-.99-.598-.6-.864-1.359-.99-2.297-.123-.91-.123-2.075-.123-3.546v-.09c0-1.47 0-2.634.123-3.546.126-.938.392-1.697.99-2.296.6-.598 1.358-.864 2.296-.99.411-.055.874-.086 1.393-.102.003-.485.015-.905.062-1.256.068-.503.216-.956.58-1.32.365-.365.819-.513 1.32-.58.477-.065 1.076-.065 1.795-.065m-2.557 3.202q.453-.002.953-.002h3.29q.501 0 .954.002a9 9 0 0 0-.052-1.077c-.05-.37-.135-.528-.24-.632s-.262-.19-.63-.24c-.387-.051-.905-.053-1.677-.053-.77 0-1.289.002-1.675.054-.37.05-.527.135-.632.24-.104.103-.19.261-.24.63-.037.277-.048.621-.051 1.078m-2.433 1.31c-.805.108-1.269.311-1.607.65-.339.338-.542.802-.65 1.607-.11.822-.112 1.906-.112 3.431s.002 2.61.112 3.432c.108.804.311 1.268.65 1.607.338.338.802.541 1.607.65.822.11 1.906.111 3.431.111h3.2c1.526 0 2.61-.001 3.432-.111.805-.109 1.268-.312 1.607-.65.338-.339.541-.803.65-1.607.11-.823.111-1.906.111-3.432 0-1.525 0-2.609-.111-3.431-.109-.805-.311-1.269-.65-1.607s-.802-.542-1.607-.65c-.822-.11-1.906-.112-3.432-.112h-3.2c-1.525 0-2.609.002-3.43.112m5.031 1.888a.6.6 0 0 1 .6.6v.008c.871.22 1.6.907 1.6 1.859a.6.6 0 1 1-1.2 0c0-.307-.34-.733-1-.733s-1 .426-1 .733.341.733 1 .733c1.108 0 2.2.768 2.2 1.934 0 .952-.729 1.639-1.6 1.858v.008a.6.6 0 1 1-1.2 0v-.008c-.87-.22-1.6-.906-1.6-1.858a.6.6 0 0 1 1.2 0c0 .307.341.733 1 .733.66 0 1-.426 1-.733 0-.308-.34-.734-1-.734-1.107 0-2.2-.768-2.2-1.933 0-.952.73-1.64 1.6-1.858v-.009a.6.6 0 0 1 .6-.6" clip-rule="evenodd"></path></svg>
                        Быстрая доставка
                    </p>
                </div>
            </div>
        </section>                  
    </main>

    <footer class="bg-black w-full py-10 mt-[100px]">
        <div class="max-w-[1440px] m-auto px-[60px] max-[768px]:px-[30px] max-[450px]:px-[15px] flex max-[850px]:flex-col max-[850px]:gap-10 justify-between items-center">
            <div>
                <h1 class=" text-[32px] max-[768px]:text-[28px] font-bold text-[#009000] tracking-widest uppercase">Yar Exchange</h1>
            </div>

            <div class=" flex gap-[350px] max-[1300px]:gap-[300px] max-[1200px]:gap-[250px] max-[1100px]:gap-[200px] max-[1040px]:gap-[150px] max-[950px]:gap-[120px] max-[900px]:gap-[90px] max-[540px]:gap-[50px] max-[500px]:gap-[30px] ">
                <div class="flex flex-col justify-between h-[130px]">
                    <a href="#calculator" class=" hover:opacity-70 transition max-[500px]:text-[14px] max-[345px]:text-[12px]" @click="buy = true">Купить криптовалюту</a>
                    <a href="#calculator" class=" hover:opacity-70 transition max-[500px]:text-[14px] max-[345px]:text-[12px]" @click="buy = false">Продать криптовалюту</a>
                    <a href="https://t.me/ADA13249" target="_blank" class="hover:opacity-70 transition max-[500px]:text-[14px] max-[345px]:text-[12px]">Сообщить об ошибке</a>
                </div>  
             
                <div class="flex flex-col h-[130px] justify-between">
                    <a href="tel:89010447605" class=" hover:opacity-70 transition max-[500px]:text-[14px]">+7 (901) 044-76-05</a>

                    <button class="bg-[#009000] bg-opacity-30 border border-[#009000] p-2 rounded-md hover:opacity-85 duration-200">
                        <a class="flex items-center gap-3 max-[500px]:gap-2 justify-center max-[500px]:text-[14px]" href="https://t.me/+QK9BS7_Uu9s4YTRi" target="_blank">
                            Сообщество
                            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 30 31" class="w-[25px] h-[25px]">
                                <g clip-path="url(#telegram_svg__a)">
                                    <path fill="#ffffff" d="M14.991.964c-8.284 0-15 6.715-15 15s6.716 15 15 15c8.285 0 15-6.716 15-15 0-8.285-6.715-15-15-15m0 .882c7.798 0 14.118 6.32 14.118 14.118 0 7.797-6.32 14.117-14.118 14.117-7.797 0-14.117-6.32-14.117-14.117S7.194 1.845 14.99 1.845m6.165 8.212c-.347-.04-.78.053-1.267.229-.889.32-12.245 5.143-12.901 5.421-.622.265-1.21.553-1.212.97 0 .294.175.459.655.63.5.178 1.758.56 2.5.765.717.197 1.532.025 1.988-.259.483-.3 6.069-4.037 6.47-4.365.4-.327.718.093.39.42-.328.329-4.16 4.049-4.666 4.564-.614.626-.178 1.274.234 1.534.47.296 3.852 2.563 4.362 2.928.51.363 1.026.529 1.5.529.472 0 .72-.623.956-1.346.276-.846 1.57-9.284 1.728-10.947.048-.503-.11-.838-.422-.987a1 1 0 0 0-.315-.086"></path>
                                </g><defs><clipPath id="telegram_svg__a"><path fill="#fff" d="M-.009.964h30v30h-30z"></path></clipPath></defs>
                            </svg>
                        </a>
                    </button>

                    <button class="bg-[#009000] bg-opacity-30 border border-[#009000] p-2 rounded-md hover:opacity-85 duration-200">
                        <a class="flex items-center gap-5 max-[500px]:gap-3 max-[500px]:justify-evenly max-[500px]:text-[14px]" href="https://wa.me/message/QNC25AFZ7NSNA1" target="_blank">
                            WhatsApp

                            <svg xmlns="http://www.w3.org/2000/svg" x="0px" y="0px" width="24" height="24" viewBox="0,0,256,256">
                                <g fill="#ffffff" fill-rule="nonzero" stroke="none" stroke-width="1" stroke-linecap="butt" stroke-linejoin="miter" stroke-miterlimit="10" stroke-dasharray="" stroke-dashoffset="0" font-family="none" font-weight="none" font-size="none" text-anchor="none" style="mix-blend-mode: normal"><g transform="scale(5.12,5.12)"><path d="M25,2c-12.69047,0 -23,10.30953 -23,23c0,4.0791 1.11869,7.88588 2.98438,11.20898l-2.94727,10.52148c-0.09582,0.34262 -0.00241,0.71035 0.24531,0.96571c0.24772,0.25536 0.61244,0.35989 0.95781,0.27452l10.9707,-2.71875c3.22369,1.72098 6.88165,2.74805 10.78906,2.74805c12.69047,0 23,-10.30953 23,-23c0,-12.69047 -10.30953,-23 -23,-23zM25,4c11.60953,0 21,9.39047 21,21c0,11.60953 -9.39047,21 -21,21c-3.72198,0 -7.20788,-0.97037 -10.23828,-2.66602c-0.22164,-0.12385 -0.48208,-0.15876 -0.72852,-0.09766l-9.60742,2.38086l2.57617,-9.19141c0.07449,-0.26248 0.03851,-0.54399 -0.09961,-0.7793c-1.84166,-3.12289 -2.90234,-6.75638 -2.90234,-10.64648c0,-11.60953 9.39047,-21 21,-21zM16.64258,13c-0.64104,0 -1.55653,0.23849 -2.30859,1.04883c-0.45172,0.48672 -2.33398,2.32068 -2.33398,5.54492c0,3.36152 2.33139,6.2621 2.61328,6.63477h0.00195v0.00195c-0.02674,-0.03514 0.3578,0.52172 0.87109,1.18945c0.5133,0.66773 1.23108,1.54472 2.13281,2.49414c1.80347,1.89885 4.33914,4.09336 7.48633,5.43555c1.44932,0.61717 2.59271,0.98981 3.45898,1.26172c1.60539,0.5041 3.06762,0.42747 4.16602,0.26563c0.82216,-0.12108 1.72641,-0.51584 2.62109,-1.08203c0.89469,-0.56619 1.77153,-1.2702 2.1582,-2.33984c0.27701,-0.76683 0.41783,-1.47548 0.46875,-2.05859c0.02546,-0.29156 0.02869,-0.54888 0.00977,-0.78711c-0.01897,-0.23823 0.0013,-0.42071 -0.2207,-0.78516c-0.46557,-0.76441 -0.99283,-0.78437 -1.54297,-1.05664c-0.30567,-0.15128 -1.17595,-0.57625 -2.04883,-0.99219c-0.8719,-0.41547 -1.62686,-0.78344 -2.0918,-0.94922c-0.29375,-0.10568 -0.65243,-0.25782 -1.16992,-0.19922c-0.51749,0.0586 -1.0286,0.43198 -1.32617,0.87305c-0.28205,0.41807 -1.4175,1.75835 -1.76367,2.15234c-0.0046,-0.0028 0.02544,0.01104 -0.11133,-0.05664c-0.42813,-0.21189 -0.95173,-0.39205 -1.72656,-0.80078c-0.77483,-0.40873 -1.74407,-1.01229 -2.80469,-1.94727v-0.00195c-1.57861,-1.38975 -2.68437,-3.1346 -3.0332,-3.7207c0.0235,-0.02796 -0.00279,0.0059 0.04687,-0.04297l0.00195,-0.00195c0.35652,-0.35115 0.67247,-0.77056 0.93945,-1.07812c0.37854,-0.43609 0.54559,-0.82052 0.72656,-1.17969c0.36067,-0.71583 0.15985,-1.50352 -0.04883,-1.91797v-0.00195c0.01441,0.02867 -0.11288,-0.25219 -0.25,-0.57617c-0.13751,-0.32491 -0.31279,-0.74613 -0.5,-1.19531c-0.37442,-0.89836 -0.79243,-1.90595 -1.04102,-2.49609v-0.00195c-0.29285,-0.69513 -0.68904,-1.1959 -1.20703,-1.4375c-0.51799,-0.2416 -0.97563,-0.17291 -0.99414,-0.17383h-0.00195c-0.36964,-0.01705 -0.77527,-0.02148 -1.17773,-0.02148zM16.64258,15c0.38554,0 0.76564,0.0047 1.08398,0.01953c0.32749,0.01632 0.30712,0.01766 0.24414,-0.01172c-0.06399,-0.02984 0.02283,-0.03953 0.20898,0.40234c0.24341,0.57785 0.66348,1.58909 1.03906,2.49023c0.18779,0.45057 0.36354,0.87343 0.50391,1.20508c0.14036,0.33165 0.21642,0.51683 0.30469,0.69336v0.00195l0.00195,0.00195c0.08654,0.17075 0.07889,0.06143 0.04883,0.12109c-0.21103,0.41883 -0.23966,0.52166 -0.45312,0.76758c-0.32502,0.37443 -0.65655,0.792 -0.83203,0.96484c-0.15353,0.15082 -0.43055,0.38578 -0.60352,0.8457c-0.17323,0.46063 -0.09238,1.09262 0.18555,1.56445c0.37003,0.62819 1.58941,2.6129 3.48438,4.28125c1.19338,1.05202 2.30519,1.74828 3.19336,2.2168c0.88817,0.46852 1.61157,0.74215 1.77344,0.82227c0.38438,0.19023 0.80448,0.33795 1.29297,0.2793c0.48849,-0.05865 0.90964,-0.35504 1.17773,-0.6582l0.00195,-0.00195c0.3568,-0.40451 1.41702,-1.61513 1.92578,-2.36133c0.02156,0.0076 0.0145,0.0017 0.18359,0.0625v0.00195h0.00195c0.0772,0.02749 1.04413,0.46028 1.90625,0.87109c0.86212,0.41081 1.73716,0.8378 2.02148,0.97852c0.41033,0.20308 0.60422,0.33529 0.6543,0.33594c0.00338,0.08798 0.0068,0.18333 -0.00586,0.32813c-0.03507,0.40164 -0.14243,0.95757 -0.35742,1.55273c-0.10532,0.29136 -0.65389,0.89227 -1.3457,1.33008c-0.69181,0.43781 -1.53386,0.74705 -1.8457,0.79297c-0.9376,0.13815 -2.05083,0.18859 -3.27344,-0.19531c-0.84773,-0.26609 -1.90476,-0.61053 -3.27344,-1.19336c-2.77581,-1.18381 -5.13503,-3.19825 -6.82031,-4.97266c-0.84264,-0.8872 -1.51775,-1.71309 -1.99805,-2.33789c-0.4794,-0.62364 -0.68874,-0.94816 -0.86328,-1.17773l-0.00195,-0.00195c-0.30983,-0.40973 -2.20703,-3.04868 -2.20703,-5.42578c0,-2.51576 1.1685,-3.50231 1.80078,-4.18359c0.33194,-0.35766 0.69484,-0.41016 0.8418,-0.41016z"></path></g></g>
                            </svg>
                        </a>
                    </button>
                </div>
            </div>
            
        </div>
    </footer>
</template>


<style lang="scss">
    input[type="number"]::-webkit-outer-spin-button,
    input[type="number"]::-webkit-inner-spin-button {
        -webkit-appearance: none;
    }

    input[type='number'],
    input[type="number"]:hover,
    input[type="number"]:focus {
        appearance: none;
        -moz-appearance: textfield;
    }

    .anim-reload {
        animation: reload 2s ease;
    }

    @keyframes reload {
    from {
        -webkit-transform: rotate(0deg);
    }
    to {
        -webkit-transform: rotate(-360deg);
    }
    }

</style>

