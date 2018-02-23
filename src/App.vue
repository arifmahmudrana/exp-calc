<template>
  <div class="container mt5e">
    <div class="panel panel-default text-center">
      <div class="panel-heading text-center">
        <h3 class="text-info">Experience Calculator</h3>
      </div>
      <div class="panel-body">
        <div class="row">
          <experience v-for="(experience, key) in experiences" :key="key">
            <div class="panel-heading" slot="heading" :title="experience.office">
              <strong class="truncate">{{ experience.office }}</strong>
            </div>
            <strong class="text-success">Started: </strong>{{ experience.start | dateFormatter }}<br />
            <strong class="text-danger">Ended: </strong>{{ experience.end | dateFormatter }}<br />
            <strong class="text-info">Experience: </strong>{{ experience | experienceFormatter }}
          </experience>
        </div>
      </div>
      <div class="panel-footer p0">
        <div class="alert alert-success text-center mb0">
          <strong>Your total experience is: </strong>{{ totalExperience }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Experience from './components/Experience'
import moment from 'moment'

const getYearMonthString = (years, months) => {
  const yearString = years > 1 ? 'years' : 'year'
  const monthString = months > 1 ? 'months' : 'month'
  years = Math.floor(years)
  months = Math.floor(months)

  if (years && months) {
    return `${years} ${yearString}, ${months} ${monthString}`
  }

  if (years && !months) {
    return `${years} ${yearString}`
  }

  if (!years && months) {
    return `${months} ${monthString}`
  }
}

export default {
  components: {
    Experience
  },

  data () {
    return {
      experiences: [
        {
          office: 'Furqan Software Ltd',
          start: 'Jul 16, 2017',
          end: ''
        },
        {
          office: 'East West Media Group Ltd(P1)',
          start: 'Feb 01, 2016',
          end: 'Jul 13, 2017'
        },
        {
          office: 'weDevs',
          start: 'Feb 01, 2015',
          end: 'Jan 31, 2016'
        },
        {
          office: 'Technobd Web Solutions (Pvt.) Ltd',
          start: 'Dec 19, 2011',
          end: 'Jan 31, 2015'
        },
        {
          office: 'Infolink',
          start: 'May 01, 2011',
          end: 'Dec 18, 2011'
        },
        {
          office: 'Webzone Technology',
          start: 'Nov 01, 2010',
          end: 'Feb 14, 2011'
        }
      ]
    }
  },

  computed: {
    totalExperience () {
      const duration = moment.duration({})
      this.experiences.forEach(experience => {
        const end = experience.end ? moment(experience.end) : moment()
        const start = moment(experience.start)
        const diff = moment.duration(end.diff(start, 'months'))

        duration.add({'M': diff})
      })

      const totalMonths = duration.asMonths()
      const months = totalMonths % 12
      const years = (totalMonths - months) / 12
      return getYearMonthString(years, months)
    }
  },

  filters: {
    dateFormatter (value) {
      if (value === '') {
        return 'Cont.'
      }

      return moment(value).format('MMM DD, YYYY')
    },

    experienceFormatter (value) {
      const end = value.end ? moment(value.end) : moment()
      const start = moment(value.start)
      const years = end.diff(start, 'year')
      start.add(years, 'years')
      const months = end.diff(start, 'months')

      return getYearMonthString(years, months)
    }
  }
}
</script>

<style>
  .mt5e {
    margin-top: 5em
  }

  .p0 {
    padding: 0
  }

  .mb0 {
    margin-bottom: 0
  }

  .truncate {
    width: 250px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }
</style>
