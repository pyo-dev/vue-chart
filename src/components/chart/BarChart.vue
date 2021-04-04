<script>
import { Bar } from 'vue-chartjs'

export default {
  extends: Bar,
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
				
				hover: {
					mode: 'nearest',
					intersect: false
				},
				layout: {
					padding: {
						right: 120
					}
				},  
				responsive: true,
				maintainAspectRatio: false,
				animation: {
					easing: 'easeInOutQuad',
					duration: 520
				},
				scales: {
					xAxes: [{
						gridLines: {
							color: 'rgba(0, 0, 0, 0.1)',
							lineWidth: 1,
						},
						ticks: {
							callback: function(value, index, values){

								return [value[0], value[1] +'월'];

							}
						},
					}],
					yAxes: [{
						gridLines: {
							color: 'rgba(0, 0, 0, 0.0)',
							lineWidth: 1,
						},
						position: 'right',
						ticks: {
              min: 2,
							fontSize: 14,
							mirror: true,
							padding: 80,
							callback: function(value, index, values){
								return value + '';
							}
						},
					}]
				},
				elements: {
					line: {
						tension: 0, // 0.4
					}
				},
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

							tooltip.dataPoints.forEach(function(data) {
								let yData = String(data.yLabel);
								yData = yData.replace(/(\d)(?=(?:\d{3})+(?!\d))/g, '$1,');
								innerHtml += '<div class="tooltip-title">' + data.xLabel[1] + '월 ' + data.xLabel[0] + '일</div>';
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
			},
  }),

  mounted () {
    this.renderChart(this.chartdata, this.chartOptions)
  }
}
</script>