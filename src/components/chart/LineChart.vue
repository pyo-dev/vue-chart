<template>
	<div>
		<canvas	as id="chartCanvas" style="height: 500px;"></canvas>
	</div>
</template>

<script>

import Chart from "chart.js";

export default {
	props: ['chartDataX', 'chartDataY'],
	data() {
		return {
			chartCanvas: null,
			chartData : {
				labels: [],
				datasets: []
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

								if(value[0] === 1 || value[0]%10 === 0){
									return [value[0], value[1] +'월'];
								} else {
									return value[0];
								}

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
			chartInstance: null,
		}
	},

	watch: {
		chartDataX(newValue, oldValue) {
			let _THIS = this;
			this.chartData.labels = this.chartDataX;
			this.chartDataY.forEach(function(v, i){
				_THIS.chartData.datasets[i].data = v;
			});
			this.chartInstance.update();
		}
	},

	mounted () {
		this.appendData();
	},

	methods: {
		appendData() {
			let _ROOT_THIS = this;
			let chart = document.getElementById('chartCanvas').getContext('2d'),
				gradient = chart.createLinearGradient(0, 0, 0, 450);
				gradient.addColorStop(0, 'rgba(0, 188, 144, 0.5)');
				gradient.addColorStop(1, 'rgba(0, 188, 144, 0.1)');
			
			this.chartData.labels = this.chartDataX;
			let realDatasets = [];
			this.chartDataY.forEach(function(v, i){
				let appData = {
					label: 'Chart',
					borderWidth: 2,
					borderColor: 'rgba(0, 188, 144, 1)',
					backgroundColor: gradient,
					pointBorderWidth: 1,
					pointRadius: 1,
					pointBorderColor: 'rgba(0, 188, 144, 0)',
					pointBackgroundColor: 'rgba(0, 188, 144, 0)',
					pointHoverRadius: 10,
					pointHoverBorderColor: 'rgba(0, 188, 144, 1)',
					pointHoverBackgroundColor: 'rgba(0, 188, 144, 0.2)',
					data: v,
				}
				realDatasets.push(appData);
			});
			this.chartData.datasets = realDatasets;
			this.chartInstance = new Chart(chart, {
				type: 'line',
				data: this.chartData,
				options: this.chartOptions
			});
		}
	},
}
</script>

<style lang="scss" scoped>

</style>