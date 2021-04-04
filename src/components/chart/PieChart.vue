<script>
import { Pie } from 'vue-chartjs'

export default {
  extends: Pie,
  data: () => ({
    chartdata: {
      labels: [['1', '12'], ['2', '12'], ['3', '12'],],
      datasets: [
        {
          label: 'Data One',
          backgroundColor: ['rgba(0, 188, 144, 1)', 'rgba(0, 188, 144, 0.6)', 'rgba(0, 188, 144, 0.3)'],
          data: [40, 20, 30]
        }
      ]
    },
    chartOptions: {
      responsive: true,
      maintainAspectRatio: false,
      legend: {
        display: false
      },
			tooltips: {
					enabled: false,
					mode: 'index',
					// mode: 'nearest',
					intersect: false,
					
					custom: function(tooltip) {
						// Tooltip Element
						let tooltipEl = document.getElementById('chartjs-tooltip');

						if (!tooltipEl) {
							tooltipEl = document.createElement('div');
							tooltipEl.id = 'chartjs-tooltip';
							tooltipEl.innerHTML = '<div class="tooltip-inner"></div>';
							// this._chart.canvas.parentNode.appendChild(tooltipEl);
							document.body.appendChild(tooltipEl);
						}

						if (tooltip.opacity === 0) {
							tooltipEl.style.opacity = 0;
							return;
						}
						
						if (tooltip.dataPoints) {

							let innerHtml = '';

              let labelData = this._data;
							tooltip.dataPoints.forEach(function(data) {
                let index = data.index;
                let xData = labelData.labels[index];
								let yData = String(labelData.datasets[0].data[index]);
								yData = yData.replace(/(\d)(?=(?:\d{3})+(?!\d))/g, '$1,');
								innerHtml += '<div class="tooltip-title">' + xData[1] + '월 ' + xData[0] + '일</div>';
								innerHtml += '<div class="tooltip-content">data <b>' + yData + '</b><span>건<span></div>';
							});

							let tableRoot = tooltipEl.querySelector('.tooltip-inner');
							tableRoot.innerHTML = innerHtml;
						}

						// `this` will be the overall tooltip
						var position = this._chart.canvas.getBoundingClientRect();

						// Display, position, and set styles for font
						tooltipEl.style.opacity = 1;
						tooltipEl.style.left = position.left + window.pageXOffset + tooltip.caretX + 'px';
						tooltipEl.style.top = position.top + window.pageYOffset + tooltip.caretY - 25 + 'px';
					}
				}
    }
  }),

  mounted () {
    this.renderChart(this.chartdata, this.chartOptions)
  }
}
</script>