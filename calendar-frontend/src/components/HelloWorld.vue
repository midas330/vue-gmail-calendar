<template>
  <div class="text-center section">
    <h2 class="h2">Custom Calendars</h2>
    <v-calendar class="custom-calendar max-w-full" :masks="masks" :attributes="attributes" disable-page-swipe
      is-expanded>
      <template v-slot:day-content="{ day, attributes }">
        <div class="flex flex-col h-full z-10 overflow-hidden">
          <span class="day-label text-sm text-gray-900">{{ day.day }}</span>
          <div class="flex-grow overflow-y-auto overflow-x-auto">
            <p v-for="attr in attributes" :key="attr.key" class="text-xs leading-tight rounded-sm p-1 mt-0 mb-1"
              :class="attr.customData.class">
              {{ attr.customData.title }}
            </p>
          </div>
        </div>
      </template>
    </v-calendar>
  </div>
</template>

<script>
  import axios from 'axios'

export default {
  data() {
   
    return {
      masks: {
        weekdays: 'WWW',
      },
      attributes: [{}],
    };
  },
  mounted() {
    axios.get('http://localhost:3000').then(res=>{
      const data = res.data.events;
      const rlt = data.map((item, index)=>({
        key: index,
        customData: {
          title: item.summary,
          class: 'bg-red-600 text-white',
        },
        dates: item.start.date
      }))
      this.attributes = rlt;
    }).catch(err=>console.log(err))
  }
  
};
</script>

<style lang="postcss" scoped>
  ::-webkit-scrollbar {
    width: 0px;
  }
  ::-webkit-scrollbar-track {
    display: none;
  }
  /deep/ .custom-calendar.vc-container {
    --day-border: 1px solid #b8c2cc;
    --day-border-highlight: 1px solid #b8c2cc;
    --day-width: 90px;
    --day-height: 90px;
    --weekday-bg: #f8fafc;
    --weekday-border: 1px solid #eaeaea;
    border-radius: 0;
    width: 100%;
    & .vc-header {
      background-color: #f1f5f8;
      padding: 10px 0;
    }
    & .vc-weeks {
      padding: 0;
    }
    & .vc-weekday {
      background-color: var(--weekday-bg);
      border-bottom: var(--weekday-border);
      border-top: var(--weekday-border);
      padding: 5px 0;
    }
    & .vc-day {
      padding: 0 5px 3px 5px;
      text-align: left;
      height: var(--day-height);
      min-width: var(--day-width);
      background-color: white;
      &.weekday-1,
      &.weekday-7 {
        background-color: #eff8ff;
      }
      &:not(.on-bottom) {
        border-bottom: var(--day-border);
        &.weekday-1 {
          border-bottom: var(--day-border-highlight);
        }
      }
      &:not(.on-right) {
        border-right: var(--day-border);
      }
    }
    & .vc-day-dots {
      margin-bottom: 5px;
    }
  }

</style>
