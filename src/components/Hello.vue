<template>
  <div>
    <x-header :right-options="{showMore: true}" @on-click-more="showMenus = true">
      天气预报－vux版
    </x-header>
    <group>
      <x-address title="请选择城市" v-model="citypost" :list="addressData" hide-district
      @on-hide="getWeather"></x-address>
    </group>
    <divider>未来五天的天气情况如下</divider>
    <group v-for="(item,index) in weather" style="align-middle">
      {{item.date}}&nbsp;{{item.fengli}}&nbsp;{{item.type}}&nbsp;{{item.low}}&nbsp;{{item.hign}}
    </group>
    <x-button type="primary" @click.native="getWeather">查询天气</x-button>
    <actionsheet :menus="menus" v-model="showMenus" show-cancel></actionsheet>
  </div>
</template>

<script>
import { Divider, Group, XButton, XAddress, XHeader, Actionsheet, ChinaAddressData, Value2nameFilter as
  value2name } from 'vux'
  
export default {
  components: {
    Divider, Group, XButton, XAddress, XHeader, Actionsheet
  },
  data () {
    return {
      baseurl: 'http://wthrcdn.etouch.cn/weather_mini?city=',
      weather: [],
      citypost: ['440000', '440100'],
      addressData: ChinaAddressData,
      menus: {
        menu1: '功能强大-点赞',
        menu2: '界面美观-点赞'
      },
      showMenus: false
    }
  },
  methods: {
    getWeather: function () {
      this.weather = []
      let url = this.baseurl + this.getName(this.citypost)
      this.$http.get(url)
        .then((res) => {
          if (res.data.status === 1000) {
            this.weather = res.data.data.forecast
          }
        })
    },
    getName (value) {
      let provinceName = value2name(value, ChinaAddressData)
      let cityName = provinceName.split(' ')[1]
      return cityName.slice(0, -1)
    }
  },
  created () {
    this.getWeather()
  }
}
</script>
