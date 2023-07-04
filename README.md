# new-product-image-swatches-shopify-liquid


{%- comment -%}
Those are the option names for which we automatically detect swatch. For the color, we use them to display a swatch, while
for size, we use it to display a size chart (if applicable)
{%- endcomment -%}

{%- assign color_label = 'color,colour,couleur,colore,farbe,색,色,färg,farve' | split: ',' -%}
{%- assign size_label = 'size,taille,größe,tamanho,tamaño,koko,サイズ' | split: ',' -%}

{%- assign size_chart_page = '' -%}
{%- assign product_popovers = '' -%}
{%- assign show_option_label = false -%}

{%- assign selected_variant = product.selected_or_first_available_variant -%}

<div class="ProductForm__Variants">
  {%- unless product.has_only_default_variant -%}
    {%- for option in product.options_with_values -%}
      {%- assign downcase_option = option.name | downcase -%}

      {%- if block.settings.selector_mode == 'block' or color_label contains downcase_option and block.settings.show_color_swatch -%}
        {%- assign show_option_label = true -%}
      {%- endif -%}
    {%- endfor -%}

   

    {%- for option in product.options_with_values -%}
      {%- assign downcase_option = option.name | downcase -%}
      {%- capture popover_id -%}popover-{{ product.id }}-{{ section.id }}-{{ option.name | handle }}{%- endcapture -%}

      <div class="ProductForm__Option {% if show_option_label %}ProductForm__Option--labelled{% endif %}">
        {%- if show_option_label -%}
          {%- assign size_chart_page = block.settings.size_chart -%}

          <span class="ProductForm__Label">
            {{ option.name }}:

            {% if color_label contains downcase_option and block.settings.show_color_swatch %}
              <span class="ProductForm__SelectedValue">{{ option.selected_value }}</span>
            {% endif %}

            {%- if size_label contains downcase_option and size_chart_page != blank -%}
              <button type="button" class="ProductForm__LabelLink Link Text--subdued" data-action="open-modal" aria-controls="modal-{{ size_chart_page.handle }}">
              {{- 'product.form.size_chart' | t -}}
            </button>
            {%- endif -%}
          </span>
        {%- endif -%}

        {%- if color_label contains downcase_option and block.settings.show_color_swatch -%}
          <ul class="ColorSwatchList HorizontalList HorizontalList--spacingTight">
            {%- assign color_swatch_config = settings.color_swatch_config | newline_to_br | split: '<br />' -%}

            {%- for value in option.values -%}
              {%- assign downcase_value = value | downcase -%}

              <li class="HorizontalList__Item">
                <input id="option-{{ section.id }}-{{ forloop.parentloop.index0 }}-{{ forloop.index0 }}" class="ColorSwatch__Radio" type="radio" name="option-{{ forloop.parentloop.index0 }}" value="{{ value | escape }}" {% if value == option.selected_value %}checked="checked"{% endif %} data-option-position="{{ option.position }}">
                <label for="option-{{ section.id }}-{{ forloop.parentloop.index0 }}-{{ forloop.index0 }}" class="ColorSwatch ColorSwatch--large {% if downcase_value == 'white' %}ColorSwatch--white{% endif %}" data-tooltip="{{ value | escape }}" style="{% render 'color-swatch-style', color_swatch_config: color_swatch_config, value: value %}">
                  <span class="u-visually-hidden">{{ value }}</span>
                </label>
              </li>
            {%- endfor -%}
          </ul>
        {%- elsif block.settings.selector_mode == 'block' -%}

    <!--  {% for variant in product.variants %}
      {% if variant.inventory_quantity == 0 %}
        0
        {% else %}
        {{ variant.inventory_quantity }}
      {% endif %}
  {% endfor %} -->
        
  <ul class="SizeSwatchList HorizontalList HorizontalList--spacingTight {% if option.name == 'Quantity' %}radioBox{% endif %} {% if option.name == 'Gift' and option1.selected_value != 'Unavailable' and option1.selected_value != '0' %}imageBox{% endif %}">
  {%- for value in option.values -%}
    {%- assign variant = product.variants | where: 'option1', value | first -%}
    <li class="HorizontalList__Item">
      <input id="option-{{ section.id }}-{{ forloop.parentloop.index0 }}-{{ forloop.index0 }}" class="SizeSwatch__Radio" type="radio" name="option-{{ forloop.parentloop.index0 }}" value="{{ value | escape }}" {% if value == option.selected_value %}checked="checked"{% endif %} data-option-position="{{ option.position }}">
      {% if option.name == 'Quantity' %} 
        <label for="option-{{ section.id }}-{{ forloop.parentloop.index0 }}-{{ forloop.index0 }}" class="SizeSwatch">
          <em></em>
          <div>
            <b>{{ value }}</b>
          <strong class="VariantPrice">{{ variant.price | money }} <span>/each</span></strong>
          </div>
        </label>
      {% elsif option.name == 'Gift'   %}
  
     

      
<label for="option-{{ section.id }}-{{ forloop.parentloop.index0 }}-{{ forloop.index0 }}" class="SizeSwatch">
    {% if product.variants[forloop.index0].inventory_quantity != 0 %}
      <div class="imgSwatches">
        <img src="{{ product.variants[forloop.index0].image | img_url: 'master' }}" height="auto" width="115px" alt="{{ value }}" class="VariantImage" data-variant-image="{{ product.variants[forloop.index0].image | img_url: 'master' }}">
      </div>
    {% else %}
      <div class="imgSwatches svg">
        <svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" aria-hidden="true" role="img" class="iconify iconify--ph" width="100%" height="100%" preserveAspectRatio="xMidYMid meet" viewBox="0 0 256 256"><path fill="currentColor" d="M208 76h-32V52a48 48 0 0 0-96 0v24H48a20.1 20.1 0 0 0-20 20v112a20.1 20.1 0 0 0 20 20h160a20.1 20.1 0 0 0 20-20V96a20.1 20.1 0 0 0-20-20ZM104 52a24 24 0 0 1 48 0v24h-48Zm100 152H52V100h152Zm-60-52a16 16 0 1 1-16-16a16 16 0 0 1 16 16Z"></path></svg>
      </div>
    {% endif %}
    <span>{{ value }}</span>
  </label>

      
      
        
      {% endif %}
    </li>
  {%- endfor -%}
</ul>


        
      
<style>

  .imageBox{ 
    display: flex;
  }
  .imageBox li{}
  .imageBox li label {
    border: 0;
    display: flex;
    flex-direction: column;
}
.imageBox li label .imgSwatches {
    border: 1px #333 dashed;
    padding: 0;
    border-radius: 5px;
    background-color: #fce1db;
    width: 140px;
    height: 140px;
    display: flex;
    align-items: center;
    justify-content: center;
}
  .imageBox li label .imgSwatches.svg{
    background-color:#e9e9e9
  }
 .imageBox li label .imgSwatches svg{
       width: 70px;
 }
 .imageBox li label img {
/*     border: 1px #333 dashed;
    padding: 10px 10px;
    border-radius: 5px;
    background-color: #fce1db; */
}
  .imageBox li label span{}
  
          .radioBox{}
        
  .radioBox li  div {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
}
  
  .radioBox li input:checked + label{
    background-color:#fbe1db
  }
  .radioBox li input:checked + label em{
    background-color:#333;
}
   .radioBox li label em {
    width: 14px;
    height: 14px;
    display: block;
    border: 2px solid #fff;
    border-radius: 50%;
    box-shadow: 0 0 0 2px #333;
}
        .radioBox li label {
    border: 1px #1c1b1b solid !important;
    display: flex !important;
    padding: 12px 23px;
    border-radius: 6px;
    align-items: center;
    gap: 10px;
}
          .radioBox li label b{
                font-size: 16px;
    font-weight: 600;
          }
          .radioBox li label strong{
            
    font-size: 14px;
    font-weight: 700;
    line-height: 1;
          }
          .radioBox li label strong span{
            
    font-weight: 400;
          }

@media(max-width: 575px){
 .radioBox li label { 
padding: 8px 18px; 
align-items: center;
gap: 5px;
}
.imageBox li label .imgSwatches { 
    width: 100px;
    height: 100px; 
}
.imageBox li label img {
    width: calc(100% - 12px);
}
.imageBox li label .imgSwatches svg {
    width: 40px;
}
.imageBox li label {
    padding: 0;
}
          }
        </style>



        
        {%- else -%}
          <button type="button" class="ProductForm__Item" aria-expanded="false" aria-controls="{{ popover_id }}">
            <span class="ProductForm__OptionName">{% unless show_option_label %}{{ option.name }}: {% endunless %}<span class="ProductForm__SelectedValue">{{ option.selected_value }}</span></span>
            {%- render 'icon' with 'select-arrow' -%}
          </button>

          {%- capture popover_html -%}
            {%- if color_label contains downcase_option and block.settings.show_color_carousel -%}
              {%- for value in option.values -%}
                {%- if value == option.selected_value -%}
                  {%- assign initial_image_index = forloop.index0 -%}
                  {%- break -%}
                {%- endif -%}
              {%- endfor -%}

              {%- capture flickity_options -%}
              {
                "prevNextButtons": true,
                "pageDots": true,
                "initialIndex": {{ initial_image_index }},
                "arrowShape": {"x0": 20, "x1": 60, "y1": 40, "x2": 60, "y2": 35, "x3": 25}
              }
              {%- endcapture -%}

              <div id="{{ popover_id }}" class="VariantSelector" aria-hidden="true">
                {%- capture option_index -%}option{{ option.position }}{%- endcapture -%}

                {%- assign option_indexes_excluded_color = '' -%}
                {%- assign selected_variant_title_excluded_color = '' -%}

                {%- for option_nested in product.options_with_values -%}
                  {%- if option_nested.position != option.position -%}
                    {%- assign option_indexes_excluded_color = option_indexes_excluded_color | append: option_nested.position | append: ',' -%}

                    {%- for option_nested_value in option_nested.values -%}
                      {%- if option_nested_value == option_nested.selected_value -%}
                        {%- assign selected_variant_title_excluded_color = selected_variant_title_excluded_color | append: option_nested_value -%}
                      {%- endif -%}
                    {%- endfor -%}
                  {%- endif -%}
                {%- endfor -%}

                {%- assign option_indexes_excluded_color = option_indexes_excluded_color | split: ',' | compact -%}

                <div class="VariantSelector__Carousel Carousel" data-flickity-config='{{ flickity_options }}'>
                  {%- for value in option.values -%}
                    <div class="VariantSelector__Item Carousel__Cell {% if value == option.selected_value %}is-selected{% endif %}"
                         data-background-color="{{ value | split: ' ' | last | handle }}"
                         data-background-image="{{ value | handle | append: '.png' | asset_url }}"
                         data-option-position="{{ option.position }}"
                         data-option-value="{{ value | escape }}">
                      {%- for variant in product.variants -%}
                        {%- if variant[option_index] == value -%}
                          {%- assign variant_image = variant.image | default: product.featured_image -%}

                          {%- assign variant_title_excluded_color = '' -%}
                          {%- for option_index_excluded_color in option_indexes_excluded_color -%}
                            {%- capture sub_option_index -%}option{{ option_index_excluded_color }}{%- endcapture -%}
                            {%- assign variant_title_excluded_color = variant_title_excluded_color | append: variant[sub_option_index] -%}
                          {%- endfor -%}

                          <div data-variant-title="{{ variant_title_excluded_color | escape }}" class="VariantSelector__ImageWrapper AspectRatio AspectRatio--withFallback" aria-hidden="{% if selected_variant_title_excluded_color == variant_title_excluded_color %}false{% else %}true{% endif %}" style="max-width: {{ variant_image.width }}px; padding-bottom: {{ 100.0 | divided_by: variant_image.aspect_ratio }}%; --aspect-ratio: {{ variant_image.aspect_ratio }}">
                            {%- capture supported_sizes -%}{%- render 'image-size', sizes: '200,400,600,800', image: variant_image -%}{%- endcapture -%}
                            {%- assign image_url = variant_image | img_url: '1x1' | replace: '_1x1.', '_{width}x.' -%}

                            <img class="VariantSelector__Image Image--lazyLoad Image--fadeIn" data-src="{{ image_url }}" data-widths="[{{ supported_sizes }}]" data-sizes="auto" alt="{{ variant_image.alt | escape }}">
                            <span class="Image__Loader"></span>
                          </div>
                        {%- endif -%}
                      {%- endfor -%}
                    </div>
                  {%- endfor -%}
                </div>

                <div class="VariantSelector__Info">
                  <div class="VariantSelector__ChoiceList">
                    {%- for value in option.values -%}
                      {%- assign min_price_for_variant = product.price_max -%}

                      {%- for variant in product.variants -%}
                        {%- if variant[option_index] == value -%}
                          {%- assign min_price_for_variant = min_price_for_variant | at_most: variant.price -%}
                        {%- endif -%}
                      {%- endfor -%}

                      <div class="VariantSelector__Choice {% if value == option.selected_value %}is-selected{% endif %}">
                        <div class="VariantSelector__ChoiceColor">
                          {%- assign downcase_value = value | downcase -%}

                          {%- assign color_swatch_name = value | handle | append: '.png' -%}
                          {%- assign color_swatch_image = images[color_swatch_name] -%}

                          <span class="VariantSelector__ColorSwatch {% if downcase_value == 'white' %}VariantSelector__ColorSwatch--white{% endif %}" style="background-color: {{ value | replace: ' ', '' | downcase }}; {% if color_swatch_image != blank %}background-image: url({{ color_swatch_image | img_url: '64x64' }}){% endif %}"></span>
                          <span class="VariantSelector__ChoiceValue">{{ value }}</span>
                        </div>

                        <div class="VariantSelector__ChoicePrice" data-color-position="{{ option.position }}">
                          {%- if available_prices_for_option_value.size > 1 -%}
                            {%- capture formatted_min_price -%}<span>{{ min_price_for_variant | money }}</span>{%- endcapture -%}
                            <span class="Heading Text--subdued">{{ 'product.form.from_price_html' | t: min_price: formatted_min_price }}</span>
                          {%- else -%}
                            <span class="Heading Text--subdued">{{ min_price_for_variant | money }}</span>
                          {%- endif -%}
                        </div>
                      </div>
                    {%- endfor -%}
                  </div>

                  <button type="button" class="VariantSelector__Button Button Button--primary Button--full" data-action="select-variant">{{- 'product.form.select_model' | t -}}</button>
                </div>
              </div>
            {%- else -%}
              <div id="{{ popover_id }}" class="OptionSelector Popover Popover--withMinWidth" aria-hidden="true">
                <header class="Popover__Header">
                  <button type="button" class="Popover__Close Icon-Wrapper--clickable" data-action="close-popover">{% render 'icon' with 'close' %}</button>
                  <span class="Popover__Title Heading u-h4">{{ option.name | escape }}</span>
                </header>

                <div class="Popover__Content">
                  <div class="Popover__ValueList" data-scrollable>
                    {%- for value in option.values -%}
                      <button type="button" class="Popover__Value {% if value == option.selected_value %}is-selected{% endif %} Heading Link Link--primary u-h6"
                              data-value="{{ value | escape }}"
                              data-option-position="{{ option.position }}"
                              data-action="select-value">
                        {{- value | escape -}}
                      </button>
                    {%- endfor -%}
                  </div>

                  {%- assign size_chart_page = block.settings.size_chart -%}

                  {%- if show_option_label == false and size_label contains downcase_option and size_chart_page != blank -%}
                    <button type="button" class="Popover__FooterHelp Heading Link Link--primary Text--subdued u-h6" data-action="open-modal" aria-controls="modal-{{ size_chart_page.handle }}">
                      {{- 'product.form.size_chart' | t -}}
                    </button>
                  {%- endif -%}
                </div>
              </div>
            {%- endif -%}
          {%- endcapture -%}

          {%- assign product_popovers = product_popovers | append: popover_html -%}
        {%- endif -%}
      </div>
    {%- endfor -%}

    <div class="no-js ProductForm__Option">
      <div class="Select Select--primary">
        {%- render 'icon' with 'select-arrow' -%}

        <select id="product-select-{{ product.id }}" name="id" title="Variant">
          {%- for variant in product.variants -%}
            <option {% if variant == selected_variant %}selected="selected"{% endif %} {% unless variant.available %}disabled="disabled"{% endunless %} value="{{ variant.id }}" data-sku="{{ variant.sku }}">{{ variant.title }} - {{ variant.price | money }}</option>
          {%- endfor -%}
        </select>
      </div>
    </div>
  {%- else -%}
    <input type="hidden" name="id" data-sku="{{ selected_variant.sku }}" value="{{ selected_variant.id }}">
  {%- endunless -%}
</div>

{%- if size_chart_page != empty -%}
  {%- comment -%}If we have a size chart we capture the modal content (it must be displayed outside the form for proper positioning){%- endcomment -%}

  {%- capture product_modals -%}
    <div id="modal-{{ size_chart_page.handle }}" class="Modal Modal--dark Modal--fullScreen Modal--pageContent" aria-hidden="true" role="dialog" data-scrollable>
      <header class="Modal__Header">
        <h2 class="Modal__Title Heading u-h1">{{ size_chart_page.title }}</h2>
      </header>

      <div class="Modal__Content Rte">
        <div class="Container Container--extraNarrow">
          {{- size_chart_page.content -}}
        </div>
      </div>

      <button class="Modal__Close RoundButton RoundButton--large" data-animate-bottom data-action="close-modal">{% render 'icon' with 'close' %}</button>
    </div>
  {%- endcapture -%}
{%- endif -%}

{%- comment -%}
--------------------------------------------------------------------------------------------------------------------
OFF SCREEN ELEMENTS

Implementation note: in the past (with "include") we could accumulate the data and output it outside the snippet
itself. However with the new "render" tag, everything that is captured inside cannot be used outside the snippet
itself. As a consequence we are moving them in JavaScript. This allows to make sure that those elements are upper
in the DOM tree, and do not show below the content
--------------------------------------------------------------------------------------------------------------------
{%- endcomment -%}

<div class="Product__OffScreen">
  {{- product_popovers -}}
  {{- product_modals -}}
</div>
