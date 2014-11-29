 $('.dragDiv').draggable({
    containment:"#activityHolder",
    start:function(event, ui){
    $(".dragCont").append( '<div class="dragDiv" style="z-index="100""><img src="./images/coin.png" width="72" height="72"></div>');
	$(this).css({'z-index':$('.dropDiv'+(count+1)+'').children().length+1});
    },
    stop:function(event, ui){
	checkIfInside($(this), $(".dropDiv"+(count+1)), ui);
	arrangeZindex();
	resetSatus();
    },
    drop:function(){
    accept: '.dropC'	
    },
    });
    
    
    function checkIfInside(coin, area, ui){
    var state = "outside";
    var orgCoin = $(ui.helper.context);
    var coinRadius = Math.max(orgCoin.outerWidth(true), orgCoin.outerHeight(true))/2;
    var areaRadius = Math.max(area.outerWidth(true), area.outerHeight(true))/2;
    var coinMidPoint = {left:ui.offset.left+coinRadius, top:ui.offset.top+coinRadius};
    var areaMidPoint = {left:area.offset().left+areaRadius, top:area.offset().top+areaRadius};
    var outerRad = areaRadius+coinRadius;
    var _xVar = coinMidPoint.left-areaMidPoint.left, _yVar = coinMidPoint.top-areaMidPoint.top;
    var xVar = Math.abs(_xVar);
    var yVar = Math.abs(_yVar);
    var coinDist = Math.sqrt(Math.pow(yVar, 2)+Math.pow(xVar, 2));
    var additionalDist = coinDist-(areaRadius-coinRadius);
    if (coinDist >= outerRad) state = "outside";
    else if (coinDist >= areaRadius) state = "outer_mid";
    else if (additionalDist > 0) state = "inner_mid";
    else state = "inner";
    var offsetLeft = ui.offset.left-area.offset().left;
    var offsetTop = ui.offset.top-area.offset().top;
    
    if (state != "outside") {
	var offsetVariaion = {left:0, top:0};
	if (additionalDist > 0) {
	    var angle = Math.atan2(_yVar, _xVar);
	    offsetVariaion.left = additionalDist*Math.cos(angle);
	    offsetVariaion.top = additionalDist*Math.sin(angle);
	}
	offsetLeft -= offsetVariaion.left;
	offsetTop -= offsetVariaion.top;
	area.append(coin);
	coin.css({"left":offsetLeft+"px", "top":offsetTop+"px"});
	
	resetSatus();
    }
    else {
	coin.remove();
    }
    applyDraggable();
    updateNC();
}
