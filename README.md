# Elementor-Widget-Slider

```
<?php

class FsultingSliderWidget extends \Elementor\Widget_Base {

	public function get_name() {
		return "TestWidget"; 
	}

	public function get_title() {
		return __("TestWidget","fsulting");
	}

	public function get_icon() {
		return 'fa fa-code'; 
	}

	public function get_categories() {
		return [ 'general' ];
	}

	protected function _register_controls() {
		$this->start_controls_section(
			'content_section',
			[
				'label' => __( 'Content', 'plugin-name' ),
				'tab' => \Elementor\Controls_Manager::TAB_CONTENT,
			]
		);
		
		$repeater = new \Elementor\Repeater();

		$repeater->add_control(
			'slide_title', [
				'label' => __( 'Slider title', 'plugin-domain' ),
				'type' => \Elementor\Controls_Manager::TEXT,
				'default' => __( 'Slide title' , 'plugin-domain' ),
				'label_block' => true,
			]
		);

		$repeater->add_control(
			'slide_content', [
				'label' => __( 'Content', 'plugin-domain' ),
				'type' => \Elementor\Controls_Manager::WYSIWYG,
				'default' => __( 'Slide Content' , 'plugin-domain' ),
				'show_label' => false,
			]
		);
		$repeater->add_control(
			'slide_image',
			[
				'label' => __( 'Slide image', 'plugin-domain' ),
				'type' => \Elementor\Controls_Manager::MEDIA,
				'show_label' => false,
			]
		);

		$this->add_control(
			'slides',
			[
				'label' => __( 'Slides', 'plugin-domain' ),
				'type' => \Elementor\Controls_Manager::REPEATER,
				'fields' => $repeater->get_controls(),
				'default' => [
					[
						'list_title' => __( 'Slide #1', 'plugin-domain' ),
						'list_content' => __( 'Slide content', 'plugin-domain' ),
					],
				
				]
				
			]
		);

		$this->end_controls_section();


		$this->start_controls_section(
			'setting_section',
			[
				'label' => __( 'Slider Settings', 'fsulting' ),
				'tab' => \Elementor\Controls_Manager::TAB_CONTENT,
			]
		);

		$this->add_control(
			'arrows',
			[
				'label' => __( 'Show arrows?', 'fsulting' ),
				'type' => \Elementor\Controls_Manager::SWITCHER,
				'label_on' => __( 'Show', 'your-plugin' ),
				'label_off' => __( 'Hide', 'your-plugin' ),
				'return_value' => 'yes',
				'default' => 'no',
			]
		);

		$this->add_control(
			'fade',
			[
				'label' => __( 'Fade Effect ?', 'fsulting' ),
				'type' => \Elementor\Controls_Manager::SWITCHER,
				'label_on' => __( 'Yes', 'your-plugin' ),
				'label_off' => __( 'No', 'your-plugin' ),
				'return_value' => 'yes',
				'default' => 'no',
			]
		);

		$this->add_control(
			'loop',
			[
				'label' => __( 'Loop?', 'fsulting' ),
				'type' => \Elementor\Controls_Manager::SWITCHER,
				'label_on' => __( 'Yes', 'your-plugin' ),
				'label_off' => __( 'No', 'your-plugin' ),
				'return_value' => 'yes',
				'default' => 'no',
			]
		);

		$this->end_controls_section();

	}

	protected function render() {

		$settings = $this->get_settings_for_display();

		if($settings['slides']){

			if(count($settings['slides']) > 1){
				if($settings['arrows'] == 'yes'){
					$arrows = 'true';
				}else{
					$arrows = 'false';
				}
				if($settings['fade'] == 'yes'){
					$fade = 'true';
				}else{
					$fade = 'false';
				}
				if($settings['loop'] == 'yes'){
					$loop = 'true';
				}else{
					$loop = 'false';
				}
				echo '<script>
						jQuery(document).ready(function($) {

							$(".banner-slider-active").slick({
								arrows: '.$arrows.',
								infinite: true,
								pauseOnHover: true,
								slidesToShow: 1,
								slideToScroll: 1,
								fade: '.$fade.',
								speed: 600,
								prevArrow: "<button class=\'cr-slick-arrow cr-slick-prev\'><i class=\'fa fa-angle-left\'></i></button>",
								nextArrow: "<button class=\'cr-slick-arrow cr-slick-next\'><i class=\'fa fa-angle-right\'></i></button>",
								loop: '.$loop.',
								
							});

						}); 

				</script>';
			}
			echo'
				<div class="banner-area">
				<div class="banner banner-slider-active">';

				foreach($settings['slides'] as $slide){

					echo '<div style="background-image:url('.wp_get_attachment_image_url($slide['slide_image']['id'], 'large').')" class="banner__single bg-image--2" data-black-overlay="6">
						<div class="container">
							<div class="row justify-content-center">
								<div class="col-xl-9 col-lg-11">
									<div class="banner__single__content text-center">
										<h1>'.$slide['slide_title'].'</h1>
										'.wpautop($slide['slide_content']).'
										<a href="contact.html" class="cr-btn">
											<span>Contact Now</span>
										</a>
									</div>
								</div>
							</div>
						</div>
					</div>';
				}		

				echo '</div>
			</div>';
		}
		

	}

	

}```
