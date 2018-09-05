# Ajax-фильтрация и сортировка для каталога товаров на InSales
Позволяет обновлять список товаров каталога без перезагрузки страницы. Смена URL без перезагрузки страницы

## Установка и настройка
Подключить скрипт на странице каталога
```
(function($) {  
  $(document).ready(function() {
	// Входящие параметры
		var load_img = 'https://assets3.insales.ru/assets/1/3537/1043921/1535717477/preloder_img.png'; // Анимация загрузки
		var filters_form = '[data-filter]'; // Форма фильтрации
    		var action = $(filters_form).attr('action'); // Урл страницы запроса (Без атрибутов)
		var sorting_select = '#sorting'; // Селект сортировки
		var product_content = '#product-content'; // Блок контента
		var product_content_items = '#product-content .cat-grid'; // Блок списка товаров
		var product_content_count = '#product-content .count'; // Блок количества товаров
		var product_content_pagination = '#product-content .advanced-pagination'; // Блок списка пагинации

  	// Настройки для плагина equalHeight
    		var equalHeightImg = '.equalHeightImg';
		var equalHeightDescr = '.equalHeightDescr';
		var equalHeight = false;
	  
	// Событие на смену фильтра
		$('filters_form').on('change', function(){
			setTimeout(function () {		  
				$(product_content).prepend('<div class="load_content"><div class="preloader"><img class="pre_hover" src="'+ load_img +'" alt=""><div class="loading-text">LOADING...</div></div></div>')
				
				var sorting = $(sorting_select).serialize();
				var serialize_str = $('filters_form').serialize();
				if(serialize_str){
					if(sorting){
						serialize_str = serialize_str + '&' + sorting;
					}
				}else{
					serialize_str = sorting;
				}
				
				var jqxhr = $.get(action +'?'+ serialize_str, function() {
					$(product_content_items).html($(jqxhr.responseText).find(product_content_items).html())
					$(product_content_count).html($(jqxhr.responseText).find(product_content_count).html())
					$(product_content_pagination).html($(jqxhr.responseText).find(product_content_pagination).html())
					
					/* Высота блока товара */
						if(equalHeight == true){
							$(equalHeightImg).equalHeights();
							$(equalHeightDescr).equalHeights();
						}
						
					/* Меняем параметры URL */
						window.history.pushState(null, null, action +'?'+ serialize_str);
						
					/* Убираем анимацию загрузки */
						$(product_content).find('.load_content').remove();
				});
			}, 1000);
		});

	// Событие на сортировку
		$(sorting_select).on('change', function(){
			$(product_content).prepend('<div class="load_content"><div class="preloader"><img class="pre_hover" src="'+ load_img +'" alt=""><div class="loading-text">LOADING...</div></div></div>')
				
			var sorting = $(sorting_select).serialize();
			var serialize_str = $('[data-filter]').serialize();
			if(serialize_str){
				if(sorting){
					serialize_str = serialize_str + '&' + sorting;
				}
			}else{
				serialize_str = sorting;
			}
				
			var jqxhr = $.get(action +'?'+ serialize_str, function() {
				$(product_content_items).html($(jqxhr.responseText).find(product_content_items).html())
					
				/* Высота блока товара */
					if(equalHeight == true){
						$(equalHeightImg).equalHeights();
						$(equalHeightDescr).equalHeights();
					}
						
				/* Меняем параметры URL */
					window.history.pushState(null, null, action +'?'+ serialize_str);
					
				/* Убираем анимацию загрузки */
					$(product_content).find('.load_content').remove();
			});
		});
	});
})(jQuery);
```

