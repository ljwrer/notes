// for http://www.dotabp.com
var getHeroList =function(){
    var list = [];
    $('.hero_row_div').slice(1).each(function(index,item){
        list = list.concat($.makeArray($(item).find('.hero')))
    });
    return list;
}
var parseHero = function($hero){
    var text = $hero.find('.radiant_hero_data span').text();
    var reg = /(\+|\-)?\d+\.?\d*/
    return parseFloat(reg.exec(text)[0])
}
var show = function(){
    var list = getHeroList();
    var heroList = list.map(function(hero){
        return new Hero($(hero))
    }).sort(function(a,b){
        return b.rate - a.rate
    }).slice(0,20).forEach(function(hero){
        console.log(hero.show())
        console.log(hero.rate);
    })
    
}
function Hero($hero){
    this.$hero = $hero;
    this.rate = parseHero($hero);
}
Hero.prototype.show = function(){
    return this.$hero.attr('herocnname')
}
show()
