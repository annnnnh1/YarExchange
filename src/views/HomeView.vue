<!-- <script setup>

import { computed, onMounted, reactive, ref, watch } from 'vue';
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
} )
</script>

<template>
    <section class="mt-[150px] flex justify-between px-[60px]">
        <div class=" w-[55%]">
            <h1 class="text-[50px] font-bold uppercase text-balance leading-[70px]">Обмен криптовалюты в <span class="text-[#009000]">России, Турции, ОАЭ, Китае, Америке</span> вместе с</h1>
            <p class="text-[50px] uppercase tracking-widest text-[#009000] font-bold">YAR EXCHANGE</p>
            <h3 class="text-[20px] mt-3">Безопасный и быстрый обмен  <span class="text-[#009000]">криптовалют</span> на наличные.</h3>
            <h3 class="text-[20px]">Наш офис находится в г. <span class="text-[#009000]">Ярославль</span>.</h3>

            <button class="bg-[#009000] py-3 rounded-md w-[400px] mt-[100px] hover:opacity-85 duration-200">
                <div class="flex items-center gap-5 justify-center text-[24px]">
                    Написать нам 
                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 30 31" class="w-[40px] h-[40px] max-[1170px]:w-[30px] max-[1170px]:h-[30px]">
                        <g clip-path="url(#telegram_svg__a)">
                            <path fill="#ffffff" d="M14.991.964c-8.284 0-15 6.715-15 15s6.716 15 15 15c8.285 0 15-6.716 15-15 0-8.285-6.715-15-15-15m0 .882c7.798 0 14.118 6.32 14.118 14.118 0 7.797-6.32 14.117-14.118 14.117-7.797 0-14.117-6.32-14.117-14.117S7.194 1.845 14.99 1.845m6.165 8.212c-.347-.04-.78.053-1.267.229-.889.32-12.245 5.143-12.901 5.421-.622.265-1.21.553-1.212.97 0 .294.175.459.655.63.5.178 1.758.56 2.5.765.717.197 1.532.025 1.988-.259.483-.3 6.069-4.037 6.47-4.365.4-.327.718.093.39.42-.328.329-4.16 4.049-4.666 4.564-.614.626-.178 1.274.234 1.534.47.296 3.852 2.563 4.362 2.928.51.363 1.026.529 1.5.529.472 0 .72-.623.956-1.346.276-.846 1.57-9.284 1.728-10.947.048-.503-.11-.838-.422-.987a1 1 0 0 0-.315-.086"></path>
                        </g><defs><clipPath id="telegram_svg__a"><path fill="#fff" d="M-.009.964h30v30h-30z"></path></clipPath></defs>
                    </svg>
                </div>
            </button>
        </div>

        <div class="w-[40%] p-4 border border-[#009000] rounded-md bg-black bg-opacity-0 backdrop-blur-sm max-h-[400px]">
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
                            <img src="../assets/img/CASHRUB.svg" alt="" class="w-[28px]">
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
                            <img src="../assets/img/USDT.svg" alt="" class="w-[28px]">
                        </p>
                    </div>
                </div>
            </div>

            <div class="mt-5 px-3">
                <div class="flex justify-between text-[14px]">
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

    <section class=" bg-[#373737] px-[60px] py-[90px] rounded-[64px] mt-[120px] w-full">
        <h1 class=" text-[40px] flex justify-center uppercase font-bold">Наши услуги</h1>

        <ul class="text-[24px] grid grid-cols-2 gap-x-[80px] gap-y-[130px] mt-20">
            <li class="flex gap-6 text-[30px] items-center text-balance justify-self-center">
                <p class="text-[50px] font-bold text-[#009000] ">01.</p>
                Обмен валют с наличной выдачей <br>или переводом на юр. лица.
            </li>

            <li class="flex gap-6 items-center text-balance justify-self-center text-[30px]">
                <p class="text-[50px] font-bold text-[#009000]">02.</p>
                Помощь в оплате за границей
            </li>

            <li class="flex gap-6 items-center text-balance justify-self-center text-[30px]">
                <p class="text-[50px] font-bold text-[#009000]">03.</p>
                Аренда карт иностранных банков
            </li>

            <li class="flex gap-6 items-center text-balance justify-self-center text-[30px]">
                <p class="text-[50px] font-bold text-[#009000]">04.</p>
                Оплата переводом за границу
            </li>
        </ul>

    </section>

    <section class="mt-[100px] px-[60px]">
        <h1 class="flex justify-center text-[40px] font-bold uppercase">Почему мы?</h1>

        <div class="grid grid-cols-4 gap-10 mt-[80px]">
            <div class="bg-black bg-opacity-0 backdrop-blur-sm p-3 rounded-lg border border-[#009000] flex justify-center">
                <p class="flex items-center gap-3">
                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 30 31" class="shrink-0 md:w-[28px] w-6 md:h-[28px] h-6"><path fill="#fff" fill-rule="evenodd" d="M12.25 12.19a2.75 2.75 0 1 1 5.5 0 2.75 2.75 0 0 1-5.5 0M15 10.94a1.25 1.25 0 1 0 0 2.5 1.25 1.25 0 0 0 0-2.5M15 15.44c-1.196 0-2.315.24-3.164.664-.803.402-1.586 1.096-1.586 2.085v.063c-.002.51-.004 1.37.81 1.96.378.273.877.447 1.495.558.623.113 1.422.17 2.445.17s1.822-.058 2.445-.17c.618-.11 1.117-.285 1.495-.559.814-.59.812-1.449.81-1.96v-.062c0-.99-.783-1.683-1.586-2.085-.849-.424-1.968-.665-3.164-.665m-3.25 2.75c0-.116.113-.422.757-.744.6-.3 1.48-.507 2.493-.507s1.894.207 2.493.507c.644.322.757.628.757.743 0 .605-.038.698-.19.807-.122.088-.373.207-.88.298-.502.09-1.203.145-2.18.145s-1.678-.055-2.18-.145c-.507-.091-.758-.21-.88-.298-.151-.11-.19-.202-.19-.807" clip-rule="evenodd"></path><path fill="#fff" fill-rule="evenodd" d="M11.723 5.24c1.444-.494 2.34-.8 3.277-.8s1.833.306 3.277.8l.727.25c1.481.507 2.625.898 3.443 1.23.412.168.767.33 1.052.496.275.16.55.358.737.625.185.263.281.588.341.9.063.324.1.713.125 1.16.048.887.048 2.102.048 3.678v1.602c0 6.1-4.608 9.026-7.348 10.223l-.027.012c-.34.148-.66.288-1.027.382-.387.099-.799.141-1.348.141-.55 0-.96-.042-1.348-.141-.367-.094-.687-.234-1.027-.382l-.027-.012C9.858 24.207 5.25 21.282 5.25 15.18v-1.602c0-1.576 0-2.791.048-3.678.025-.447.062-.836.125-1.16.06-.312.156-.637.34-.9.188-.267.463-.466.738-.625.285-.166.64-.328 1.052-.496.818-.332 1.962-.723 3.443-1.23zm3.277.7c-.658 0-1.305.211-2.92.764l-.572.196c-1.513.517-2.616.895-3.39 1.21a7 7 0 0 0-.864.403 2 2 0 0 0-.208.14.4.4 0 0 0-.055.05.4.4 0 0 0-.032.073q-.03.082-.063.248a7 7 0 0 0-.1.958c-.046.841-.046 2.016-.046 3.624v1.575c0 5.175 3.87 7.722 6.449 8.848.371.163.586.254.825.316.228.058.506.094.976.094s.748-.036.976-.094c.24-.062.454-.153.825-.316 2.58-1.126 6.449-3.673 6.449-8.848v-1.575c0-1.608 0-2.783-.046-3.624a7 7 0 0 0-.1-.958 2 2 0 0 0-.063-.248.4.4 0 0 0-.032-.073.4.4 0 0 0-.055-.05 2 2 0 0 0-.208-.14 7 7 0 0 0-.864-.403c-.774-.315-1.877-.693-3.39-1.21l-.573-.196c-1.614-.553-2.261-.765-2.919-.765" clip-rule="evenodd"></path></svg>
                    Конфиденциальность
                </p>
            </div>

            <div class="bg-black bg-opacity-0 backdrop-blur-sm p-3 rounded-lg border border-[#009000] flex justify-center">
                <p class="flex items-center gap-3">
                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 25 25" class="shrink-0 md:w-[28px] w-6 md:h-[28px] h-6"><path fill="#fff" fill-rule="evenodd" d="M14.565 6.139h-3.72a3 3 0 0 1-2.34 2.34v.52a3 3 0 0 1 2.34 2.34h3.72a3 3 0 0 1 2.34-2.34v-.52a3 3 0 0 1-2.34-2.34m.483-1.198q-.33-.004-.702-.003h-3.283q-.372 0-.702.003a.6.6 0 0 0-.13.002 9 9 0 0 0-.962.06c-.502.067-.956.215-1.32.58-.364.364-.513.818-.58 1.32-.038.278-.053.6-.06.961a.6.6 0 0 0-.002.13q-.003.33-.002.703v.083q0 .372.002.702a.6.6 0 0 0 .002.13c.007.363.022.684.06.962.067.502.216.956.58 1.32s.818.513 1.32.58c.278.038.6.053.961.06a.6.6 0 0 0 .131.002q.33.004.702.003h3.283q.373 0 .702-.003a.6.6 0 0 0 .13-.002c.363-.007.684-.022.962-.06.502-.067.956-.216 1.32-.58s.513-.818.58-1.32c.038-.278.054-.6.06-.961a.6.6 0 0 0 .002-.13q.004-.33.003-.703v-.083q0-.372-.003-.702a.6.6 0 0 0-.002-.13 9 9 0 0 0-.06-.962c-.067-.502-.215-.956-.58-1.32-.364-.365-.818-.513-1.32-.58a9 9 0 0 0-.961-.06.6.6 0 0 0-.13-.002m.772 1.233c.182.483.566.867 1.05 1.05l-.019-.161c-.05-.37-.135-.527-.24-.632s-.262-.19-.63-.24zm1.05 4.08a1.8 1.8 0 0 0-1.05 1.05q.084-.009.16-.019c.37-.05.527-.135.632-.24s.19-.262.24-.63zm-7.28 1.05a1.8 1.8 0 0 0-1.05-1.05q.008.084.018.16c.05.37.135.527.24.632s.262.19.631.24zm-1.05-4.08a1.8 1.8 0 0 0 1.05-1.05q-.085.007-.161.018c-.37.05-.527.135-.631.24-.105.104-.19.262-.24.63zm4.165 1.314a.2.2 0 1 0 0 .4.2.2 0 0 0 0-.4m-1.4.2a1.4 1.4 0 1 1 2.8 0 1.4 1.4 0 0 1-2.8 0m-1.253 5.959c1.492-.29 3.09-.224 4.383.547.182.108.352.243.5.41.301.338.457.758.464 1.178q.229-.148.462-.326l1.446-1.092a2.11 2.11 0 0 1 2.499 0c.669.505.935 1.411.456 2.179-.34.545-.852 1.3-1.374 1.782-.527.489-1.277.9-1.845 1.173-.689.333-1.433.517-2.157.634-1.48.24-3.019.203-4.481-.099a11.4 11.4 0 0 0-2.292-.234H6.305a.6.6 0 1 1 0-1.2h1.808c.85 0 1.707.089 2.534.259a11.3 11.3 0 0 0 4.047.09c.663-.108 1.283-.268 1.828-.53.547-.265 1.161-.612 1.55-.973.395-.365.836-.998 1.172-1.538.102-.162.088-.397-.161-.585a.91.91 0 0 0-1.053 0l-1.446 1.092c-.577.436-1.287.902-2.168 1.043a7 7 0 0 1-.42.054c-.45.045-.972.055-1.546 0a.6.6 0 1 1 .113-1.194c.501.048.952.037 1.334-.002l.023-.002a.594.594 0 0 0 .12-.91 1 1 0 0 0-.22-.178c-.944-.564-2.206-.658-3.54-.4a9.7 9.7 0 0 0-3.628 1.547.6.6 0 1 1-.694-.979 10.9 10.9 0 0 1 4.094-1.746" clip-rule="evenodd"></path></svg>
                    Выгодный курс
                </p>
            </div>
            <div class="bg-black bg-opacity-0 backdrop-blur-sm p-3 rounded-lg border border-[#009000] flex justify-center">
                <p class="flex items-center gap-3">
                    <svg version="1.1" fill="white" xmlns="http://www.w3.org/2000/svg" class="shrink-0 w-5 h-5" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px" viewBox="0 0 512 512" xml:space="preserve"><path d="M176,472c5.52,0,10-4.48,10-10s-4.48-10-10-10s-10,4.48-10,10S170.48,472,176,472z"/><path d="M66,482c0,16.542,13.458,30,30,30h320c16.542,0,30-13.458,30-30V222c0-16.542-13.458-30-30-30h-10v-42 C406,67.29,338.71,0,256,0S106,67.29,106,150v42H96c-16.542,0-30,13.458-30,30V482z M126,372H86v-40h40V372z M86,392h40v40H86V392z M126,312H86v-40h40V312z M386,332h40v40h-40V332z M426,312h-40v-40h40V312z M386,392h40v40h-40V392z M126,150 c0-71.682,58.318-130,130-130s130,58.318,130,130v42h-40v-42c0-49.626-40.374-90-90-90s-90,40.374-90,90v42h-40V150z M186,192v-42 c0-38.598,31.402-70,70-70s70,31.402,70,70v42H186z M96,212h320c5.514,0,10,4.486,10,10v30h-50c-5.523,0-10,4.477-10,10v180 c0,5.523,4.477,10,10,10h50v30c0,5.514-4.486,10-10,10H96c-5.514,0-10-4.486-10-10v-30h50c5.523,0,10-4.477,10-10V262 c0-5.523-4.477-10-10-10H86v-30C86,216.486,90.486,212,96,212z"/><path d="M215.99,471H336c5.523,0,10-4.477,10-10s-4.477-10-10-10H215.99c-5.523,0-10,4.477-10,10S210.467,471,215.99,471z"/><path d="M256,371.29c-4.272,0-8.884-2.687-12.986-7.565c-3.554-4.227-9.862-4.774-14.089-1.219 c-4.228,3.554-4.773,9.862-1.219,14.089c5.346,6.359,11.632,10.79,18.294,13.024v11.671c0,5.523,4.477,10,10,10s10-4.477,10-10 v-11.72c11.639-4.128,20-15.243,20-28.28c0-16.542-13.458-30-30-30c-5.514,0-10-4.486-10-10s4.486-10,10-10 c3.542,0,7.283,1.807,10.816,5.227c3.97,3.841,10.3,3.737,14.14-0.233c3.84-3.969,3.736-10.3-0.233-14.14 c-5.075-4.911-10.153-7.688-14.724-9.205V281.29c0-5.523-4.477-10-10-10s-10,4.477-10,10v11.72c-11.639,4.128-20,15.243-20,28.28 c0,16.542,13.458,30,30,30c5.514,0,10,4.486,10,10S261.514,371.29,256,371.29z"/></svg>
                    Безопасные сделки
                </p>
            </div>
            <div class="bg-black bg-opacity-0 backdrop-blur-sm p-3 rounded-lg border border-[#009000] flex justify-center">
                <p class="flex items-center gap-3">
                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 25 25" class="shrink-0 md:w-[28px] w-6 md:h-[28px] h-6"><path fill="#fff" fill-rule="evenodd" d="M12.833 3.945h.083c.719 0 1.318 0 1.794.064.502.068.956.216 1.32.58.364.365.513.818.58 1.32.048.352.06.772.063 1.257.52.016.982.047 1.393.102.938.126 1.697.392 2.295.99.6.599.865 1.358.99 2.296.123.912.123 2.076.123 3.546v.09c0 1.47 0 2.635-.122 3.547-.126.938-.392 1.697-.99 2.295-.6.599-1.358.865-2.296.99-.912.123-2.076.123-3.546.123h-3.29c-1.47 0-2.635 0-3.547-.122-.938-.126-1.697-.392-2.296-.99-.598-.6-.864-1.359-.99-2.297-.123-.91-.123-2.075-.123-3.546v-.09c0-1.47 0-2.634.123-3.546.126-.938.392-1.697.99-2.296.6-.598 1.358-.864 2.296-.99.411-.055.874-.086 1.393-.102.003-.485.015-.905.062-1.256.068-.503.216-.956.58-1.32.365-.365.819-.513 1.32-.58.477-.065 1.076-.065 1.795-.065m-2.557 3.202q.453-.002.953-.002h3.29q.501 0 .954.002a9 9 0 0 0-.052-1.077c-.05-.37-.135-.528-.24-.632s-.262-.19-.63-.24c-.387-.051-.905-.053-1.677-.053-.77 0-1.289.002-1.675.054-.37.05-.527.135-.632.24-.104.103-.19.261-.24.63-.037.277-.048.621-.051 1.078m-2.433 1.31c-.805.108-1.269.311-1.607.65-.339.338-.542.802-.65 1.607-.11.822-.112 1.906-.112 3.431s.002 2.61.112 3.432c.108.804.311 1.268.65 1.607.338.338.802.541 1.607.65.822.11 1.906.111 3.431.111h3.2c1.526 0 2.61-.001 3.432-.111.805-.109 1.268-.312 1.607-.65.338-.339.541-.803.65-1.607.11-.823.111-1.906.111-3.432 0-1.525 0-2.609-.111-3.431-.109-.805-.311-1.269-.65-1.607s-.802-.542-1.607-.65c-.822-.11-1.906-.112-3.432-.112h-3.2c-1.525 0-2.609.002-3.43.112m5.031 1.888a.6.6 0 0 1 .6.6v.008c.871.22 1.6.907 1.6 1.859a.6.6 0 1 1-1.2 0c0-.307-.34-.733-1-.733s-1 .426-1 .733.341.733 1 .733c1.108 0 2.2.768 2.2 1.934 0 .952-.729 1.639-1.6 1.858v.008a.6.6 0 1 1-1.2 0v-.008c-.87-.22-1.6-.906-1.6-1.858a.6.6 0 0 1 1.2 0c0 .307.341.733 1 .733.66 0 1-.426 1-.733 0-.308-.34-.734-1-.734-1.107 0-2.2-.768-2.2-1.933 0-.952.73-1.64 1.6-1.858v-.009a.6.6 0 0 1 .6-.6" clip-rule="evenodd"></path></svg>
                    Быстрая доставка
                </p>
            </div>
        </div>
    </section>
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

</style> -->
<template>1</template>