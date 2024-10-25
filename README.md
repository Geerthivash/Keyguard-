# Keyguard 
A software that enhances children's online safety by restricting access to inappropriate contents.

## mail.py

```python
import os
import pyautogui
import easygui
import pygetwindow as gw
from pynput.keyboard import Key, Listener
import logging
import threading
from mail import email_alert
import datetime
import tkinter as tk
from tkinter import messagebox
import socket
import Security_Key
# Set up logging for keystrokes
log_dir = ""
logging.basicConfig(filename=(log_dir + "keylogs.txt"),
                    level=logging.DEBUG, format='%(message)s')
#Security key file creation
Security_Key.generate_time_based_key()

# List of banned words
banned_words=['nob', 'pimp', 'gfy', 'mong', 'pornhub.com','xvideos.com','xhamster','omegle.com','nipples', 'apeshit', 'golden shower', 'splooge moose', 'yeasty', 'faig', 'extacy', 'dlck', 'dagos', 'damage', 'shitstain', 'kinky', 'cop some wood', 'gonorrehea', 'muthafuckker', 'lolita', 'opiate', 'milf', 'ejaculatings', 'f4nny', 'fuc', 'package', 'pust', 'asshead', 'lech', 'ar5e', 'barenaked', 'asshore', 'butt-bang', 'fetish', 'assblaster', 'chodes', 'nonce', 'fingerfucking', 'dingleberry', 'x-rated', 'horniest', 'anal impaler', 'zoophilia', 'unspoiled', 'jaggi', 'stupid', 'smash', 'bra', 'alabama hot pocket', 'atrocity', 'ejakulate', 'hebe', 'dumbass', 'ritard', 'l3i+ch', 'booobs', 'holy shit', 'cipa', 'beatyourmeat', 'willies', 'assholes', 'sleazy', 'fuck', 'eat my ass', 'virgin', 'kum', 'breastlover', 'felching', 'herp', 'c-u-n-t', 'cuntlicking', 'revue', 'agony', 'fukwit', 'bomd', 'cockbite', 'octopussy', 'mothafuckas', 'knobjokey', 'woody', 'fuckheads', 'mothafucker', 'fucknutt', 'muffdiving', 'omg', 'moolie', 'fudge-packer', 'masterbating', 'shag', 'phukked', 'niggers', 'cleveland steamer', 'tainted love', 'valium', 'unwed', 'cuntfuck', 'lovemaking', 'snowballing', 'kooch', 'crabs', 'shiteater', 'cockburger', 'dickhead', 'pole dance', 'iberian slap', 'wazoo', 'hookah', 'foad', 'b17ch', 'dirty pillows', 'shiznit', 'debase', 'brown showers', 'knobend', 'mthrfucking', 'pubic', 'butthole', 'dillweed', 'freefuck', 'asscock', 'ball sack', 'cretin', 'pot', 'dork', 'clit licker', 'hoare', 'bosomy', 'paki', 'annihilation', 'mutha', 'semen', 'negro', 'gook', 'bareback', 'mof0', 'homoerotic', 'contraception', 'cervix', 'penial', 'jock', 'aeolus', 'damnation', 'titt', 'smartasses', 'beef curtain', 'nazi', 'coons', 'dullard', 'bint', 'scissoring', 'pubis', 'queers', 'orgasm', 'giant cock', 'lesbian', 'doggie style', 'd0ng', 'clown', 'cocaine', 'rumprammer', 'virginal', 'suicide', 'big tits', 'danger', 'wanker', 'anal', 'd1ld0', 'aryan', 'knockers', 'fellatio', 'spic', 'rectal', 'jackhole', 'sand nigger', 'bogan', 'wh0reface', 'shit', 'middle finger', 'd1ck', 'quicky', 'beef curtains', 'tight white', 'panty', 'bowels', 'dike', 'mothafucka', 'ejaculating', 'sucking', 'moo moo foo foo', 'pole smoker', 'bunghole', 'hobag', 'fastfuck', 'tool', 'anal leakage', 'sex', 'son-of-a-bitch', 'cum dumpster', 'fanyy', 'girls gone wild', 'nobjocky', 'bulldike', 'disaster', 'fucktoy', 'organ', 'va-j-j', 'fingering', 'terrorize', 'shithole', 'niggle', 'pissers', 'cums', 'rubber', 'mams', 'queef', 'ass juice', 'tit wank', 'fatfuck', 'bullshitted', 'pedophile', 'bbw', 'genocide', 'nsfw images', 'cumbubble', 'hiv', 'blow', 'deggo', 'cumdump', 'azz', 'hoor', 'bong', 'brunette action', 'subjugate', 'ball gag', 'imperil', 'hoe', 'tongue in a', 'heeb', 'hurt', 'dumass', 'bloody', 'breastjob', 'heartache', 'strap on', 'frigga', 'weewee', 'child-fucker', 'foot fetish', 'fuckhole', 'lmao', 'boink', 'masturbate', 'assfaces', 'anguish', 'frigg', 'wh0re', 'cuntfucker', 'corksucker', 'kunilingus', 'masterbate', 'cracker', 'goddamn', 'm-fucking', 'taste my', 'dickweasel', 'diligaf', 'hymen', 'orgies', 'dominate', 'cocksmoker', 'babes', 'fingerfuck', 'lube', 'commie', 'thundercunt', 'prostitution', 'leper', 'phuking', 'beeyotch', 'mound of venus', 'smutty', 'missionary position', 'm0fo', 'orgy', 'coerce', 'bumclat', 'ruination', 'cum', 'cock pocket', 'peepee', 'mothafuck', 'red light district', 'dumbasses', 'dildo', 'dink', 'fuckmehard', 'feltcher', 'slap and tickle', 'motherfuckka', 'dirty sanchez', 'fooker', 'boned', 'masterbat', 'bummer', 'shite', 'massacre', 'pusse', 'asshopper', 'guido', 'beastial', 'coksucka', 'assmuncher', 'gaybob', 'spoil', 'nude', 'lezbian', 'penetration', 'bloodclaat', 'angelic', 'cunt', 'hot chick', 'ponyplay', 'maim', 'ball kicking', 'kock', 'huge fat', 'dog-fucker', 'assmucus', 'smegma', 'fagging', 'fuckher', 'lezzies', 'pissed off', 'cunthunter', 'blight', 'c.o.c.k.', 'arsehole', 'mothafuckers', 'sucked', 'poonani', 'wtf', 'cockjockey', 'biatch', 'asswipe', 'penisfucker', 'wrapping men', 'bootie', 'mothafuckin', 'shaved beaver', 'd0uche', 'sh!+', 'shrimping', 'fudgepacker', 'assshole', 'virgin mary', 'sod off', 'tramp', 'whored', 'gokkun', 'sexo', 'buttmunch', 'smut', 'bodily', 'cunillingus', 'balls', 'assclown', 'fannyfucker', 'wedgie', 'ejaculates', 'asskisser', 'fat', 'shitt', 'catastrophe', 'yaoi', 'sausage queen', 'wrinkled starfish', 'damned', 'dickslap', 'fuckwhore', 'crap', 'enslave', 'cocksuckers', 'blow job', 'venus mound', 'turd', 'injure', 'cockface', 'cok', 'pure', 'cuntsicle', 'diddle', 'titfuck', 'pedophilia', 'numbnuts', 'asshole', 'domination', 'testis', 'sexting', 'floozy', 'seamen', 'butt fuck', 'pornos', 'vag', 'pedo', 'shaved pussy', 'puss', 'cocksuck', 'dolcett', 'rtard', 'wiener', 'bigtits', 'motherfucked', 'shitty', 'jerk', 'cyberfucking', 'pedobear', 'twinkie', 'gippo', 'assjacker', 'analsex', 'prostitute', 'eliminate', 'fagged', 'wad', 'asses', 'lusting', 'mothafucked', 'cut rope', 'vestal', 'raping', 'penispuffer', 'kikes', 'kike', 'booty', 'boong', 'kinbaku', 'nigg4h', 'whoring', 'foobar', 'bisexual', 'mutherfucker', "bang (one's) box", 'shitting', 'dickbrain', 'faggit', 'gtfo', 'lameass', 'shithead', 'testes', 'jap', 'lezbians', 'booger', 'pissing', 'cunt-struck', 'extermination', 'footfucker', 'orgasms', 'goddamned', 'shitings', 'doochbag', 'fucktard', 'spick', 'dawgie-style', 'mick', 'bimbos', 'cum guzzler', 'faigt', 'gassy ass', 'goatcx', 'faggot', 'splooge', 'f.u.c.k', 'titty', 'loin', 'punta', 'spik', 'leather restraint', 'shitter', 'breeder', 'dick-ish', 'disfigure', 'steamy', 'boonga', 'bombing', 'doublelift', 'buttbang', 'shitbreath', 'beater', 'undefiled', 'pikey', 'fuckmonkey', 'whiz', 'w00se', 'sodomize', 'deface', 'raped', 'coital', 'bitch tit', 'fucktwat', 'fistfuck', 'douchebags', 'neonazi', 'kooches', 'sh1t', 'clitoris', 'spade', 'tittiefucker', 'cyberfuc', 'gay sex', 'rosy palm and her 5 sisters', 'chesticle', 'cummin', 'bigbutt', 'fukkin', 'fuckboy', 'pegging', 'baby juice', 'babeland', 'cockmuncher', 'nads', 'dickheads', 'beaners', 'grope', 'group sex', 'gooch', 'butt-pirate', 'ham flap', 'bangbros', 'idiot', 'bampot', 'prig', 'strip club', 'jizm', 'ovum', 'choc ice', 'reverse cowgirl', 'coprophilia', 'sadness', 'bazongas', 'ass-jabber', 'assmaster', 'clunge', 'extasy', 'santorum', 'corrupt', 'virginity', 'booooooobs', 'punany', 'dick hole', 'beatch', 'asslover', 'butchdike', 'motherfuckings', 'misuse', 'humping', 'shaggin', 'bazooms', 'buttmuncher', 'eat a dick', 'fuckstick', 'dingle', 'dickripper', 'nappy', 'bone', 'fux0r', 'ass-hat', 'bosom', 'clover clamps', 'schlong', 'tranny', 'raging boner', 'crikey', 'cokmuncher', 'unclefucker', 'stiffy', 'violate', 'porch monkey', 'rape', 'pinko', 'twunter', 'whoralicious', '2 girls 1 cup', 'shit ass', 'buttcheeks', 'kafir', 'spunk', 'scrotum', 'jerked', 'execution', 'dingleberries', 'dinks', 'torture', 'dickdipper', 'fvck', 'bull shit', 'tart', 'spook', 'beastiality', 'cunthole', 'h0m0', 'rusty trombone', 'suicide girls', 'dimwit', 'clogwog', 'jackass', 'make me come', 'phallic', 'butthead', 'rubbish', 'maxi', 'frotting', 'skullfuck', 'inbred', 'phuks', 'two girls one cup', 'junky', 'fannyflaps', 'raunch', 'xx', 'orgasim', 'kunja', 'cocksucked', 'honkey', 'knobed', 'git', 'fuckhead', 'lust', 'cockholster', 'bawdy', 'fellate', 'snuff', 'crush', 'ass-fucker', 'female squirting', 'cock', 'fuk', 'fcuk', 'fisty', 'assho1e', 'banging', 'nob jokey', 'kunt', 'heroin', 'cock sucker', 'a55hole', 'carnage', 'coffin dodger', 'fistfucks', 'fucked', 'ejaculate', 'shited', 'teez', 'masterbat3', 'kondums', 'date rape', 'nigga', 'kondum', 'dickmonger', 'assbite', 'junkie', 'how to murdep', 'dickhole', 'mutherfucking', 'cockmonkey', 'exterminate', 'piss-off', 'felcher', 'beaver lips', 'cunn', 'shitbagger', 'strappado', 'ass', 'homodumbshit', 'lesbos', 'dirty', 'playboy', 'masterb8', 'sissy', 'douche-fag', 'nutsack', 'cahone', 'asshat', 'coon', 'teets', 'analprobe', 'heshe', 'nudity', 'devastate', 'hazard', 'virgo', 'masterbations', 'gayfuck', 'polack', 'spiks', 'seks', 'nut sack', 'wanky', 'tinkle', 'girl on top', 'whorehouse', 'beaver', 'motherfucks', 'douch3', 'f-u-c-k', 'coochie', 'doosh', 'essohbee', 'shitdick', 'sniper', 'lesbians', 'desecrate', 'douchewaffle', 'hard on', 'fuks', 'threaten', 'poopchute', 'paddy', 'throating', 'titi', 'v1gra', 'r-tard', 'jiz', 'pms', 'camwhore', 'weed', 'assbag', 'lezzie', 'ejaculation', 'ballsack', 'tushy', 'faggotcock', 'fatass', 'shitfuck', 'pastie', 'munter', 'cumming', 'peril', 'lardass', 'damnit', 'pee', 'niggah', 'jigaboo', 'dickflipper', 'bender', 'pollute', 'phuked', 'assshit', 'bigger', 'f u c k e r', 'dammit', 'cocklover', 'goldenshower', 'l3itch', 'nimrod', 'masturbating', 'nig-nog', 'cuntlick', 'fucker', 'fag', 'darkie', 'bitcher', 'lez', 'two fingers', 'nobhead', 'ballbag', 'taint', 'bullshit', 'buceta', 'faggitt', 'cockmongler', 'smeg', 'seduce', 'klan', 'risk', 'clitorus', 'goregasm', 'jerkass', 'pecker', 'scag', 'terrorism', 'prude', 'fuckwhit', 'suffering', 'white power', 'jerk0ff', 'fcuking', 'degenerate', 'deepthroat', 'dp action', 'fuker', 'jungle bunny', 'ahole', 'vorarephilia', 'fuck buttons', 'hell', 'asswhore', 'strapon', 'looney', 'motherfucker', 'boob', 'retarded', 'webcam', 'phukking', 'bastards', 'raper', 'bukkake', 'ass hole', 'fleshflute', 'shit fucker', 'fuckbutt', 'sperm', 'minge', 'wigger', 'blowjob', 'pawn', 'pansy', 'muff puff', 'buttfuck', 'chota bags', 'fuckface', 'zubb', 'calamity', 'cooter', 'brassiere', 'damn', 'camgirl', 'booby', 'sh!t', 'boang', 'climax', 'vagina', 'c-o-c-k', 'pollock', 'chick with a dick', 'bumfuck', 'cyberfuckers', 'cocktease', 'muff diver', 'boobjob', 'poonany', 'fuck trophy', 'bellend', 'b00bs', 'whoreface', 'chode', 'twathead', '2g1c', 'nipple', 'cocksucks', 'herpes', 'bung hole', 'sambo', 'strip', 'batty boy', 'nuts', 'ass-pirate', 'bigbastard', 'asscracker', 'knobbing', 'cyberfucker', 'rapist', 'blow mud', 'dickwhipper', 'whorealicious', 'seaman', 'hom0', 'bum boy', 'whoar', 'assbang', 'gai', 'virginize', 'assault', 'gaysex', 'assbagger', 'shitted', 'piss off', 'prince albert piercing', 'fisted', 'gringo', 'slope', 'vixen', 'douchebag', 'one cup two girls', 'fingerfuckers', 'phuq', 'bloodshed', 'screw', 'fucking', 'boobies', 'cocksmoke', 'towelhead', 'bollick', 'innocent', 'fucktards', 'ma5terbate', 'bi+ch', 'punani', 'knob', 'pissed', 'swinger', 'fuck hole', 'profane', 'muthafecker', 'arrse', 'shirt lifter', 'fuckfreak', 'cumshot', 'doofus', 'dookie', 'jiggaboo', 'fistfuckers', 'bitch', 'jeopardy', 'cuntsucker', 'vajayjay', 'big black', 'clitty litter', 'erect', 'blumpkin', 'fucknugget', 'decimate', 'juggs', 'uzi', 'butchdyke', 'cocksukka', 'bang', 'fuckersucker', 'god damn', 'fuckmeat', 'shitbag', 'lamebrain', 'dicksucker', 'loins', 'muff', 'tush', 'guro', 'wank', 'donkey punch', 'male squirting', 'bollok', 'kootch', 'fubar', 'rimming', 'butt plug', 'threesome', 'pervert', 'h0mo', 'bitchin', 'nutter', 'asswhole', 'shota', 'dickwad', 'pleasure chest', 'shitspitter', 'box', 'fartknocker', 'incest', 'nooky', 'fuckbutter', 'sluts', 'deprave', 'gaytard', 'he11', 'scantily', 'titties', 'cocain', 'bonehead', 'bicurious', 'jackoff', 'doggie-style', 'extinguish', 'cumshots', 'nambla', 'wet dream', 'cockmaster', 'assgoblin', 'rapey', 'knobhead', 'ho', 'imbecile', 'pussy', 'reefer', 'bookie', 'disable', 'masturbation', 'bollock', 'kumming', 'wench', 'bunny fucker', 'need the dick', 'cocknugget', 'massa', 'skank', 'polesmoker', 'fuckers', 'hentai', 'molest', 'assbangs', 'cunny', 'shagger', 'shi+', 'assassination', 'fagfucker', 'potty', 'dominatrix', 'muffdiver', 'nobjokey', 'footjob', 'porn', 'affliction', 'crack', 'dopey', 'fuckme', 'pcp', 'shittiest', 'assbandit', 'wound', 'woe', 'queerbait', 'dickbag', 'pussy juice', 'cockeye', 'hemp', 'intact', 'assklown', 'sucks', 'cum chugger', 'spread legs', 'ninny', 'bohunk', 'mr hands', 'footfuck', 'asspacker', 'tyrannize', 'hootch', 'rum', 'peckerhead', 'figging', 'xrated', 'headfuck', 'cuntface', 'fuckings', 'slut bucket', 'upskirt', 'hitler', 'gangbangs', 'lmfao', 'bitching', 'cummer', 'footlicker', 'tittyfuck', 'whores', 'fuckingshitmotherfucker', 'kyke', 'how to kill', 'jelly donut', 'pussys', 'creampie', 'fagots', 'pisser', 'fistfucker', 'mothafucks', 'piss pig', 'opium', 'how to murder', 'humped', 'anus', 'breastman', 'nut butter', 'pimpis', 'fanny', 'asskiss', 'drunk', 'chink', 'fudge packer', 'twatwaffle', 'gspot', 'chocolate rosebuds', 'dummy', 't1tt1e5', 'fxck', 'ovums', 'paedophile', 'cameltoe', 't1tties', 'buggered', 'breasts', 'fuck-bitch', 'asswipes', 'cornhole', 'hot carl', 'arse', 'ovary', 'dick head', 'dildos', 'fagot', 'oral', 'cockknocker', 'dendrophilia', 'doggy style', 'kinkster', 'whorehopper', 'dyke', 'assfucker', 'urinal', 'bullshits', 'fingerfucked', 'dicktickler', 'pissin', 'renob', 'twink', 'hardcoresex', 'cumslut', 'bod', 'stoned', 'darn', 'numskull', 'clam', 'cocksucking', 'scrog', 'teat', 'pubic hair', 'retard', 'barfface', 's-h-1-t', 'cockmunch', 'flog the log', 'queerhole', 'sanger', 'fart', 'assface', 'a54', 'son of a motherless goat', 'fist fuck', 'fool', 'sumofabiatch', 'pissflaps', 'fuck-ass', 'cuntlicker', 'fukwhit', 'gang-bang', 'fuq', 'nazism', 'contaminate', 'psycho', 'herpy', 'dicksucking', 'boners', 'fuckin', 'tw4t', 'doggin', 'rack', 'camboy', 'boozy', 'toke', 'gigolo', 'vibrator', 'gaylord', 'stroke', 'facefucker', 'hard core', 'fuckoff', 'camel toe', 'viagra', 'assfukka', 'cuntslut', 'escort', 'poop', 's hit', 'titwank', 'oppress', 'taig', 'shitfaced', 'bi-sexual', 'lesbo', 'niggas', 'assmonkey', 'blowjobs', 'prod', 'skeet', 'weenie', 'violet wand', 'goo girl', 'chi-chi man', 'cocklump', 'facial', '5hit', 'jiggerboo', 'foah', 'deep throat', 'shagging', 'dickweed', 'asswad', 'gae', 'twatlips', 'shitcanned', 'dickface', 'eatpussy', 'fuck-tard', 'voyeur', 'jizzed', 't1t', 'queer', 'pricks', 'quim', 'banger', 'nymphomania', 'orally', 'bastinado', 'twats', 'cybersex', 'spac', 'goatse', 'tard', 'fuckyou', 'butchery', 'he-she', 'g-spot', 'phuck', 'sexual', 'jugs', 'bung', 'bondage', 'bulldyke', 'naked', 'mothafuckings', 'gangbanged', 'dipshit', 'menage a trois', 'fack', 'intimidate', 'poon', 'tears', 'cocksucker', 'mother fucker', 'hore', 'honkers', 'cocklicker', 'dick', 'hooter', 'suckass', 'fannybandit', 'fondle', 'perversion', 'tub girl', 'peyote', 'b!tch', 'hotpussy', 'tampon', 'sandbar', 'assbanger', 'scat', 'buttman', 'womb', 'poop chute', 'sadism', 'porno', 'menses', 'dirsa', 'mothafuckaz', 'moron', 'trumped', 'carpetmuncher', 'dvda', 'hoar', 'p.u.s.s.y.', 'pasty', 'sodomy', 'bum', 'vulva', 'bastard', 'kraut', 'fuckwad', 'whorebag', 'c0cksucker', 'beaver cleaver', 'choad', 'feck', 'porchmonkey', 'raghead', 'grief', 'dickmilk', 'meth', 'masochist', 'erotic', 'jizz', 'chincs', 'freakyfucker', 'vodka', 'nigg3r', 'sodom', 'cox', 'cuntrag', 'areole', 'bumblefuck', 'wang', 'jagoff', 'gonad', 'panties', 'brotherfucker', 'shittier', 'misery', 'injun', 'labia', 'ugly', 'goddammit', 'transsexual', 'orgasims', 'mongoloid', 'beardedclam', 'shitface', 'cockwaffle', 'jism', 'two fingers with tongue', 'slag', 'dipship', 'piss', 'rimjob', 'gender bender', 'one guy one jar', 'son of a bitch', 'bastardo', 'penisbanger', 'cockshit', 'feltch', 'freakfuck', 'urophilia', 'cumjockey', 'pubes', 'member', 'breast', 'intercourse', 'war', 'dago', 'shits', 'hooch', 'shiz', 'tied up', 'buffoon', 'tea bagging', 'duche', 'dommes', 'testicle', 'wop', 'waste', 'fecal', 'tittyfucker', 'boozer', 'camslut', 'asssucker', 'rump', 'doggystyle', 's.h.i.t.', 'cyalis', 'wee-wee', 'maiden', 'motherfuckers', 'caca', 'fecker', 'jerkoff', 'chaste', 'circlejerk', 'fuck you', 'assjockey', 'gooks', 'sleaze', 'knobjocky', 'call girl', 'dong', 'mtherfucker', 'scroat', 'wiseasses', 'pussy palace', 'cunilingus', 'babe', 'cyberfuck', 'coprolagnia', 'menstruation', 'cocksucer', 'kums', 'how to make a bomb','tubgirl', 'birdlock', 'weirdo', 's_h_i_t', 'brothel', 'asslicker', 'cumguzzler', 'foreskin', 'lezbo', 'fistfuckings', 'pedophiliac', 'butt-fuck', 'dickfucker', 'godsdamn', 'god-dam', 'assnigger', 'xxx', 'homo', 'undies', 'shittings', 'buttface', 'kwif', 'shitfull', 'crappy', 'freaking', 'prickteaser', 's-h-i-t', 'blow me', 'jack-off', 'cuntass', 'twunt', 'arian', 'birth control', 'donkeypunch', 'twat', 'blue waffle', 'puto', 'bootee', 'booooobs', 'fistfucking', 'slaughterhouse', 'c0ck', 'untouched', 'assfuck', 'barface', 'defile', 'autoerotic', 'cripple', 'destroy', 'threat', 'clitface', 'whore', 'brutality', 'futanari', 'nympho', 'pigfucker', 'bully', 'tities', 'topless', 'pisses', 'bugger', 'buttplug', 'scum', 'wreck', 'pussypounder', 'maidenly', 'cockblock', 'handjob', 'omorashi', 'lemon party', 'reetard', 'p0rn', 'pussi', 'condom', 'gash', 'doggiestyle', 'wankjob', 'ghay', 'pain', 'toots', 'gang bang', 'clit', 'b1tch', 'donkeyribber', 'blow your load', 'screwing', 'd1ldo', 'vomit', 'lezzy', 'motherfucking', 'snatch', 'slaughter', 'cnut', 'motherfuckin', 'whitey', 'cockass', 'thug', 'abuse', 'punkass', 'queero', 'c.0.c.k', 'urethra play', 'lezbos', 'freex', 'cunntt', 'bunga', 'debauch', 'busty', 'boiolas', 'homoey', 'fenian', 'clits', 'rosy palm', 'ecchi', 'bitchtits', 'eat hair pie', 'shitblimp', 'old bag', 's-o-b', 'fux', 'd0uch3', 'urine', 'pantie', 'goddamnit', 'knob end', 'kill', 'cocksmith', 'cockhead', '5h1t', 'bestiality', 'hoer', 'motherfuck', 'booze', 'fuckedup', 'terd', 'chinky', 'nad', 'dumb ass', 'endanger', 'doggy-style', 'reich', 'cunnie', 'goddam', 'bust a load', 's.o.b.', 'menstruate', 'balllicker', 'flamer', 'coonnass', 'sexy', 'son of a whore', 'gey', 'poof', 'gayfuckist', 'buggery', 'yobbo', 'zoophile', 'hooker', 'nincompoop', 'double dong', 'buttfucka', 'dumbshit', 'menace', 'choade', 'phalli', 'torment', 'kitty', 'fingerfucker', 'dickjuice', 'jack off', 'vjayjay', 'dog style', 'dogging', 'pube', 'wetback', 'smartass', 'bitchers', 'fingerbang', 'assbanged', 'ginger', 'yellow showers', 'cumdumpster', 'muthafuckaz', 'swastika', 'enlargement', 'kummer', 'schizo', 'gotohell', 'wog', 'mofo', 'ball licking', 'ejaculated', 'crack-whore', 'fags', 'genitals', 'violence', 'taff', 'fcuker', 'trashy', 'god-damned', 'blonde on blonde action', 'minger', 'crackwhore', 'punky', 'fukkers', 'clitfuck', 'hotsex', 'shamedame', 'a2m', 'lezza', 'sultry women', 'thrust', 'slave', 'nig nog', 'fuckingbitch', 'sod', 'faggots', 'areola', 'racy', 'beotch', 'a_s_s', 'asspirate', 'f_u_c_k', 'dry hump', 'iap', 'cocks', 'mo-fo', 'teabagging', 'homey', 'mcfagget', 'douchey', 'barf', 'cunnilingus', 'fucknut', 'penetrate', 'big knockers', 'lap dance', 'fuck puppet', 'harm', 'masterbation', 'destruction', 'a$$hole', 'fook', 'motherfucka', 'shitey', 'fistfucked', 'japs', 'booty call', 'flaps', 'ma5terb8', 'nigaboo', 'kkk', 'spotless', 'panooch', 'clusterfuck', 'jeopardize', 'a$$', 'nawashi', 'jailbait', 'gaydo', 'shitbrains', 'double penetration', 'asspuppies', 'axwound', 'muthrfucking', 'eunuch', 'mafugly', 'screwed', 'pussy fart', 'bullturds', 'fuck off', 'midget', 'cock snot', 'bimbo', 'scrud', 'bullcrap', 'gays', 'hand job', 'girl on', 'dumbbell', 'buttfucker', 'dickfuck', 'gaywad', 'knobead', 'chastity', 'cum freak', 'bitchy', 'boner', 'mutilate', 'coochy', 'pussylicking', 'tittywank', 'bitchass', 'phonesex', 'cuntbag', 'fucks', 'black cock', 'dickzipper', 'cumtart', 'bitches', 'alaskan pipeline', 'yid', 'shibari', 'nitwit', 'testee', 'm45terbate', 'chin', 'honky', 'shithouse', 'assh0le', 'cocksuka', 'queaf', 'veqtable', 'crotte', 'junglebunny', 'boooobs', 'butt', 'dickish', 'souse', 'butt-fucker', 'boobs', 'gonads', 'testical', 'willy', 'fucka', 'hooters', 'dumbcunt', 'carpet muncher', 'pisspig', 's&m', 'sandnigger', 'tosser', 'dick-sneeze', 'flange', 'dicks', 'shitters', 'pthc', 'slutkiss', 'slutbag', 'hardcore', 'eradicate', 'godamn', 'slut', 'm0f0', 'penile', 'fuckup', 'uterus', 'barely legal', 'tawdry', 'melons', 'jerk off', 'n1gger', 'cl1t', 'virginality', 'dick shy', 'taking the piss', 'bollox', 'shiting', '4r5e', 'shitass', 'niglet', 'pornography', 'gayass', 'pron', 'sandler', 'fuck yo mama', 'kawk', 'hussy', 'ganja', 'cockknoker', 'cumstain', 'fingerfucks', 'dickwod', 'felch', 'rimjaw', 'yiffy', 'hump', 'teste', 'dumbfuck', 'fuckass', 'assmunch', 'pussies', 'cawk', 'bullet vibe', 'pissoff', 'cunts', 'cockfucker', 'n1gga', 'despair', 'ruski', 'ball sucking', 'horny', 'tribadism', 'v14gra', 'chinc', 'fagbag', 'window licker', 'munging', 'orgasmic', 'twatty', 'penis', 'stfu', 'tits', 'fuckwitt', 'shemale', 'mthrfucker', 'hun', 'sadist', 'goddamnmuthafucker', 'undressing', 'ghey', 'ball gravy', 'azazel', 'cocksniffer', 'phuk', 'buttmuch', 'dumshit', 'glans', 'fagtard', 'cyberfucked', 'demolish', 'spooge', 'lusty', 'f u c k', 'piece of shit', 'cock-sucker', 'anilingus', 'homicide', 'shitheads', 'fucktart', 'weiner', 'beaner', 'c.u.n.t', 'j3rk0ff', 'murder', 'mothafucking', 'scrot', 'tit', 'slanteye', 'shitcunt', 'ass fuck', 'c-0-c-k', 'vulgar', 'erection', 'scrote', 'daterape', 'clitty', 'pillowbiter', 'cunt hair', 'napalm', 'fatfucker', 'a55', 'blockhead', 'fuckbag', 'beer', 'baby batter', 'fukker', 'faggs', 'bdsm', 'bollocks', 'corp whore', 'dunce', 'niggaz', 'slutdumper', 'exploit', 'virtue', 'bitched', 'master-bate', 'bestial', 'fuckbrain', 'muther', 'badfuck', 'leather straight jacket', 'assranger', 'bitchez', 'big breasts', 'femdom', 'obliterate', 'gangbang', 'bloody hell', 'prick', 'jackasses', 'poopuncher', 'wiseass', 'nigger', 'jerk-off', 'rectum', 'punanny', 'blonde action', 'dykes', 'fuckwit', 'tittie5', 'assman', 'rectus', 'nimphomania', 'cocknose', 'bowel', 'poontang', 'godamnit', 'jail bait', 'asslick', 'fagg', 's0b', 'style doggy', 'erotism', 'dickbeaters', 'analannie', 'suck', '69', 'ruin', 'golliwog', 'soused', 'cockmongruel', 'skag', 'douche', 'goodpoop', 'phone sex', 'dicksipper', 'auto erotic', 'fisting']

attempt_count = 0

file_path = './keylogs.txt'

# Get absolute path
abs_file_path = os.path.abspath(file_path)
# Function to handle key presses
current_word = ""

dt = datetime.datetime.now()
dt_str = dt.strftime("%Y-%m-%d %H:%M:%S")

device_name = socket.gethostname()

print(file_path)
def show_warning():
    root = tk.Tk()
    root.withdraw()  # Hide the root window
    root.attributes("-topmost", True)  # Ensure the dialog appears on top
    root.grab_set()  # Grab the focus
    messagebox.showwarning("Warning", "You have entered banned words in your browser\nAs a result, your access to this service has been restricted.")
    root.mainloop()

def show_alert():
    root = tk.Tk()
    root.withdraw()  # Hide the root window
    root.attributes("-topmost", True)  # Ensure the dialog appears on top
    root.grab_set()  # Grab the focus
    messagebox.showinfo("Alert", "Your PC will be shut down if you continue to search for inappropriate content.")
    root.mainloop()

def show_shut():
    root = tk.Tk()
    root.withdraw()  # Hide the root window
    root.attributes("-topmost", True)  # Ensure the dialog appears on top
    root.grab_set()  # Grab the focus
    messagebox.showerror("Shutdown Alert", "Your system is going to shut down!")
    root.mainloop()

def on_press(key):
    global current_word
    # Log only the keystrokes
    if hasattr(key, 'char'):
        char = key.char
        current_word += char
        # Save the keystroke
        # logging.info(char)
    elif key == Key.space  or key == Key.tab:
        current_word+=" "
        # Check the word when space, enter, or tab is pressed
    elif key == Key.enter :
        logging.info(current_word)
        check_word(current_word.strip())
        current_word = ""  # Reset current word
    elif key == Key.backspace:
        # Handle backspace
        current_word = current_word[:-1]


# Function to check the word against the list of banned words
def close_br():
    global attempt_count
    windows = gw.getAllWindows()
    attempt_count += 1
    for window in windows:
        if "chrome" in window.title.lower() or "edge" in window.title.lower() or "firefox" in window.title.lower() or "brave" in window.title.lower() or "tor" in window.title.lower() or "brave" in window.title.lower():
            # Close the window if it's a browser window
            window.close()
    # Display appropriate message and take action based on attempt count
    if attempt_count == 1:
         #title = "Warning: Banned Words Detected"
         # message = "\t\tYou have entered banned words in your browser\n\nAs a result, your access to this service has been restricted.\nPlease refrain from using inappropriate language to avoid further action."
         threading.Thread(target=show_warning).start()
         #d_box(attempt_count)'

    elif attempt_count == 2:
        # title = "ALERT"
        # message = "\tYour PC will be shut down if you continue to search for inappropriate content."
        threading.Thread(target=show_alert).start()
        #d_box(attempt_count)

    elif attempt_count==3:
        threading.Thread(target=show_shut).start()
        email_alert("ALERT: Inappropriate Online Activity Detected",
                    "\n\tWe regret to inform you that we have detected potentially concerning online activity involving your child. Our monitoring system has flagged inappropriate content being accessed or searched for.\n\nDetails:\n\nDate and Time: {}\nDevice name:{}\nActivity: This device has searched for the phrase {} in the browser, indicating potential engagement with inappropriate"
                    " content.".format(dt_str,device_name, current_word), "geerthi006@gmail.com", file_path)
        os.system("shutdown /s /t 1")

def check_word(word):
    global attempt_count
    windows = gw.getAllWindows()

    for banned_word in banned_words:
        if banned_word in word.lower():
                close_br()





# Start listening for keystrokes
with Listener(on_press=on_press) as listener:
    try:
        listener.join()  # Start the listener
    except KeyboardInterrupt:
        print("Keylogger stopped by user.")
        listener.stop()
```

## keyguard.py:

```py
import os
import pyautogui
import easygui
import pygetwindow as gw
from pynput.keyboard import Key, Listener
import logging
import threading
from mail import email_alert
import datetime
import tkinter as tk
from tkinter import messagebox
import socket
import Security_Key
# Set up logging for keystrokes
log_dir = ""
logging.basicConfig(filename=(log_dir + "keylogs.txt"),
                    level=logging.DEBUG, format='%(message)s')
#Security key file creation
Security_Key.generate_time_based_key()

# List of banned words
banned_words=['nob', 'pimp', 'gfy', 'mong', 'pornhub.com','xvideos.com','xhamster','omegle.com','nipples', 'apeshit', 'golden shower', 'splooge moose', 'yeasty', 'faig', 'extacy', 'dlck', 'dagos', 'damage', 'shitstain', 'kinky', 'cop some wood', 'gonorrehea', 'muthafuckker', 'lolita', 'opiate', 'milf', 'ejaculatings', 'f4nny', 'fuc', 'package', 'pust', 'asshead', 'lech', 'ar5e', 'barenaked', 'asshore', 'butt-bang', 'fetish', 'assblaster', 'chodes', 'nonce', 'fingerfucking', 'dingleberry', 'x-rated', 'horniest', 'anal impaler', 'zoophilia', 'unspoiled', 'jaggi', 'stupid', 'smash', 'bra', 'alabama hot pocket', 'atrocity', 'ejakulate', 'hebe', 'dumbass', 'ritard', 'l3i+ch', 'booobs', 'holy shit', 'cipa', 'beatyourmeat', 'willies', 'assholes', 'sleazy', 'fuck', 'eat my ass', 'virgin', 'kum', 'breastlover', 'felching', 'herp', 'c-u-n-t', 'cuntlicking', 'revue', 'agony', 'fukwit', 'bomd', 'cockbite', 'octopussy', 'mothafuckas', 'knobjokey', 'woody', 'fuckheads', 'mothafucker', 'fucknutt', 'muffdiving', 'omg', 'moolie', 'fudge-packer', 'masterbating', 'shag', 'phukked', 'niggers', 'cleveland steamer', 'tainted love', 'valium', 'unwed', 'cuntfuck', 'lovemaking', 'snowballing', 'kooch', 'crabs', 'shiteater', 'cockburger', 'dickhead', 'pole dance', 'iberian slap', 'wazoo', 'hookah', 'foad', 'b17ch', 'dirty pillows', 'shiznit', 'debase', 'brown showers', 'knobend', 'mthrfucking', 'pubic', 'butthole', 'dillweed', 'freefuck', 'asscock', 'ball sack', 'cretin', 'pot', 'dork', 'clit licker', 'hoare', 'bosomy', 'paki', 'annihilation', 'mutha', 'semen', 'negro', 'gook', 'bareback', 'mof0', 'homoerotic', 'contraception', 'cervix', 'penial', 'jock', 'aeolus', 'damnation', 'titt', 'smartasses', 'beef curtain', 'nazi', 'coons', 'dullard', 'bint', 'scissoring', 'pubis', 'queers', 'orgasm', 'giant cock', 'lesbian', 'doggie style', 'd0ng', 'clown', 'cocaine', 'rumprammer', 'virginal', 'suicide', 'big tits', 'danger', 'wanker', 'anal', 'd1ld0', 'aryan', 'knockers', 'fellatio', 'spic', 'rectal', 'jackhole', 'sand nigger', 'bogan', 'wh0reface', 'shit', 'middle finger', 'd1ck', 'quicky', 'beef curtains', 'tight white', 'panty', 'bowels', 'dike', 'mothafucka', 'ejaculating', 'sucking', 'moo moo foo foo', 'pole smoker', 'bunghole', 'hobag', 'fastfuck', 'tool', 'anal leakage', 'sex', 'son-of-a-bitch', 'cum dumpster', 'fanyy', 'girls gone wild', 'nobjocky', 'bulldike', 'disaster', 'fucktoy', 'organ', 'va-j-j', 'fingering', 'terrorize', 'shithole', 'niggle', 'pissers', 'cums', 'rubber', 'mams', 'queef', 'ass juice', 'tit wank', 'fatfuck', 'bullshitted', 'pedophile', 'bbw', 'genocide', 'nsfw images', 'cumbubble', 'hiv', 'blow', 'deggo', 'cumdump', 'azz', 'hoor', 'bong', 'brunette action', 'subjugate', 'ball gag', 'imperil', 'hoe', 'tongue in a', 'heeb', 'hurt', 'dumass', 'bloody', 'breastjob', 'heartache', 'strap on', 'frigga', 'weewee', 'child-fucker', 'foot fetish', 'fuckhole', 'lmao', 'boink', 'masturbate', 'assfaces', 'anguish', 'frigg', 'wh0re', 'cuntfucker', 'corksucker', 'kunilingus', 'masterbate', 'cracker', 'goddamn', 'm-fucking', 'taste my', 'dickweasel', 'diligaf', 'hymen', 'orgies', 'dominate', 'cocksmoker', 'babes', 'fingerfuck', 'lube', 'commie', 'thundercunt', 'prostitution', 'leper', 'phuking', 'beeyotch', 'mound of venus', 'smutty', 'missionary position', 'm0fo', 'orgy', 'coerce', 'bumclat', 'ruination', 'cum', 'cock pocket', 'peepee', 'mothafuck', 'red light district', 'dumbasses', 'dildo', 'dink', 'fuckmehard', 'feltcher', 'slap and tickle', 'motherfuckka', 'dirty sanchez', 'fooker', 'boned', 'masterbat', 'bummer', 'shite', 'massacre', 'pusse', 'asshopper', 'guido', 'beastial', 'coksucka', 'assmuncher', 'gaybob', 'spoil', 'nude', 'lezbian', 'penetration', 'bloodclaat', 'angelic', 'cunt', 'hot chick', 'ponyplay', 'maim', 'ball kicking', 'kock', 'huge fat', 'dog-fucker', 'assmucus', 'smegma', 'fagging', 'fuckher', 'lezzies', 'pissed off', 'cunthunter', 'blight', 'c.o.c.k.', 'arsehole', 'mothafuckers', 'sucked', 'poonani', 'wtf', 'cockjockey', 'biatch', 'asswipe', 'penisfucker', 'wrapping men', 'bootie', 'mothafuckin', 'shaved beaver', 'd0uche', 'sh!+', 'shrimping', 'fudgepacker', 'assshole', 'virgin mary', 'sod off', 'tramp', 'whored', 'gokkun', 'sexo', 'buttmunch', 'smut', 'bodily', 'cunillingus', 'balls', 'assclown', 'fannyfucker', 'wedgie', 'ejaculates', 'asskisser', 'fat', 'shitt', 'catastrophe', 'yaoi', 'sausage queen', 'wrinkled starfish', 'damned', 'dickslap', 'fuckwhore', 'crap', 'enslave', 'cocksuckers', 'blow job', 'venus mound', 'turd', 'injure', 'cockface', 'cok', 'pure', 'cuntsicle', 'diddle', 'titfuck', 'pedophilia', 'numbnuts', 'asshole', 'domination', 'testis', 'sexting', 'floozy', 'seamen', 'butt fuck', 'pornos', 'vag', 'pedo', 'shaved pussy', 'puss', 'cocksuck', 'dolcett', 'rtard', 'wiener', 'bigtits', 'motherfucked', 'shitty', 'jerk', 'cyberfucking', 'pedobear', 'twinkie', 'gippo', 'assjacker', 'analsex', 'prostitute', 'eliminate', 'fagged', 'wad', 'asses', 'lusting', 'mothafucked', 'cut rope', 'vestal', 'raping', 'penispuffer', 'kikes', 'kike', 'booty', 'boong', 'kinbaku', 'nigg4h', 'whoring', 'foobar', 'bisexual', 'mutherfucker', "bang (one's) box", 'shitting', 'dickbrain', 'faggit', 'gtfo', 'lameass', 'shithead', 'testes', 'jap', 'lezbians', 'booger', 'pissing', 'cunt-struck', 'extermination', 'footfucker', 'orgasms', 'goddamned', 'shitings', 'doochbag', 'fucktard', 'spick', 'dawgie-style', 'mick', 'bimbos', 'cum guzzler', 'faigt', 'gassy ass', 'goatcx', 'faggot', 'splooge', 'f.u.c.k', 'titty', 'loin', 'punta', 'spik', 'leather restraint', 'shitter', 'breeder', 'dick-ish', 'disfigure', 'steamy', 'boonga', 'bombing', 'doublelift', 'buttbang', 'shitbreath', 'beater', 'undefiled', 'pikey', 'fuckmonkey', 'whiz', 'w00se', 'sodomize', 'deface', 'raped', 'coital', 'bitch tit', 'fucktwat', 'fistfuck', 'douchebags', 'neonazi', 'kooches', 'sh1t', 'clitoris', 'spade', 'tittiefucker', 'cyberfuc', 'gay sex', 'rosy palm and her 5 sisters', 'chesticle', 'cummin', 'bigbutt', 'fukkin', 'fuckboy', 'pegging', 'baby juice', 'babeland', 'cockmuncher', 'nads', 'dickheads', 'beaners', 'grope', 'group sex', 'gooch', 'butt-pirate', 'ham flap', 'bangbros', 'idiot', 'bampot', 'prig', 'strip club', 'jizm', 'ovum', 'choc ice', 'reverse cowgirl', 'coprophilia', 'sadness', 'bazongas', 'ass-jabber', 'assmaster', 'clunge', 'extasy', 'santorum', 'corrupt', 'virginity', 'booooooobs', 'punany', 'dick hole', 'beatch', 'asslover', 'butchdike', 'motherfuckings', 'misuse', 'humping', 'shaggin', 'bazooms', 'buttmuncher', 'eat a dick', 'fuckstick', 'dingle', 'dickripper', 'nappy', 'bone', 'fux0r', 'ass-hat', 'bosom', 'clover clamps', 'schlong', 'tranny', 'raging boner', 'crikey', 'cokmuncher', 'unclefucker', 'stiffy', 'violate', 'porch monkey', 'rape', 'pinko', 'twunter', 'whoralicious', '2 girls 1 cup', 'shit ass', 'buttcheeks', 'kafir', 'spunk', 'scrotum', 'jerked', 'execution', 'dingleberries', 'dinks', 'torture', 'dickdipper', 'fvck', 'bull shit', 'tart', 'spook', 'beastiality', 'cunthole', 'h0m0', 'rusty trombone', 'suicide girls', 'dimwit', 'clogwog', 'jackass', 'make me come', 'phallic', 'butthead', 'rubbish', 'maxi', 'frotting', 'skullfuck', 'inbred', 'phuks', 'two girls one cup', 'junky', 'fannyflaps', 'raunch', 'xx', 'orgasim', 'kunja', 'cocksucked', 'honkey', 'knobed', 'git', 'fuckhead', 'lust', 'cockholster', 'bawdy', 'fellate', 'snuff', 'crush', 'ass-fucker', 'female squirting', 'cock', 'fuk', 'fcuk', 'fisty', 'assho1e', 'banging', 'nob jokey', 'kunt', 'heroin', 'cock sucker', 'a55hole', 'carnage', 'coffin dodger', 'fistfucks', 'fucked', 'ejaculate', 'shited', 'teez', 'masterbat3', 'kondums', 'date rape', 'nigga', 'kondum', 'dickmonger', 'assbite', 'junkie', 'how to murdep', 'dickhole', 'mutherfucking', 'cockmonkey', 'exterminate', 'piss-off', 'felcher', 'beaver lips', 'cunn', 'shitbagger', 'strappado', 'ass', 'homodumbshit', 'lesbos', 'dirty', 'playboy', 'masterb8', 'sissy', 'douche-fag', 'nutsack', 'cahone', 'asshat', 'coon', 'teets', 'analprobe', 'heshe', 'nudity', 'devastate', 'hazard', 'virgo', 'masterbations', 'gayfuck', 'polack', 'spiks', 'seks', 'nut sack', 'wanky', 'tinkle', 'girl on top', 'whorehouse', 'beaver', 'motherfucks', 'douch3', 'f-u-c-k', 'coochie', 'doosh', 'essohbee', 'shitdick', 'sniper', 'lesbians', 'desecrate', 'douchewaffle', 'hard on', 'fuks', 'threaten', 'poopchute', 'paddy', 'throating', 'titi', 'v1gra', 'r-tard', 'jiz', 'pms', 'camwhore', 'weed', 'assbag', 'lezzie', 'ejaculation', 'ballsack', 'tushy', 'faggotcock', 'fatass', 'shitfuck', 'pastie', 'munter', 'cumming', 'peril', 'lardass', 'damnit', 'pee', 'niggah', 'jigaboo', 'dickflipper', 'bender', 'pollute', 'phuked', 'assshit', 'bigger', 'f u c k e r', 'dammit', 'cocklover', 'goldenshower', 'l3itch', 'nimrod', 'masturbating', 'nig-nog', 'cuntlick', 'fucker', 'fag', 'darkie', 'bitcher', 'lez', 'two fingers', 'nobhead', 'ballbag', 'taint', 'bullshit', 'buceta', 'faggitt', 'cockmongler', 'smeg', 'seduce', 'klan', 'risk', 'clitorus', 'goregasm', 'jerkass', 'pecker', 'scag', 'terrorism', 'prude', 'fuckwhit', 'suffering', 'white power', 'jerk0ff', 'fcuking', 'degenerate', 'deepthroat', 'dp action', 'fuker', 'jungle bunny', 'ahole', 'vorarephilia', 'fuck buttons', 'hell', 'asswhore', 'strapon', 'looney', 'motherfucker', 'boob', 'retarded', 'webcam', 'phukking', 'bastards', 'raper', 'bukkake', 'ass hole', 'fleshflute', 'shit fucker', 'fuckbutt', 'sperm', 'minge', 'wigger', 'blowjob', 'pawn', 'pansy', 'muff puff', 'buttfuck', 'chota bags', 'fuckface', 'zubb', 'calamity', 'cooter', 'brassiere', 'damn', 'camgirl', 'booby', 'sh!t', 'boang', 'climax', 'vagina', 'c-o-c-k', 'pollock', 'chick with a dick', 'bumfuck', 'cyberfuckers', 'cocktease', 'muff diver', 'boobjob', 'poonany', 'fuck trophy', 'bellend', 'b00bs', 'whoreface', 'chode', 'twathead', '2g1c', 'nipple', 'cocksucks', 'herpes', 'bung hole', 'sambo', 'strip', 'batty boy', 'nuts', 'ass-pirate', 'bigbastard', 'asscracker', 'knobbing', 'cyberfucker', 'rapist', 'blow mud', 'dickwhipper', 'whorealicious', 'seaman', 'hom0', 'bum boy', 'whoar', 'assbang', 'gai', 'virginize', 'assault', 'gaysex', 'assbagger', 'shitted', 'piss off', 'prince albert piercing', 'fisted', 'gringo', 'slope', 'vixen', 'douchebag', 'one cup two girls', 'fingerfuckers', 'phuq', 'bloodshed', 'screw', 'fucking', 'boobies', 'cocksmoke', 'towelhead', 'bollick', 'innocent', 'fucktards', 'ma5terbate', 'bi+ch', 'punani', 'knob', 'pissed', 'swinger', 'fuck hole', 'profane', 'muthafecker', 'arrse', 'shirt lifter', 'fuckfreak', 'cumshot', 'doofus', 'dookie', 'jiggaboo', 'fistfuckers', 'bitch', 'jeopardy', 'cuntsucker', 'vajayjay', 'big black', 'clitty litter', 'erect', 'blumpkin', 'fucknugget', 'decimate', 'juggs', 'uzi', 'butchdyke', 'cocksukka', 'bang', 'fuckersucker', 'god damn', 'fuckmeat', 'shitbag', 'lamebrain', 'dicksucker', 'loins', 'muff', 'tush', 'guro', 'wank', 'donkey punch', 'male squirting', 'bollok', 'kootch', 'fubar', 'rimming', 'butt plug', 'threesome', 'pervert', 'h0mo', 'bitchin', 'nutter', 'asswhole', 'shota', 'dickwad', 'pleasure chest', 'shitspitter', 'box', 'fartknocker', 'incest', 'nooky', 'fuckbutter', 'sluts', 'deprave', 'gaytard', 'he11', 'scantily', 'titties', 'cocain', 'bonehead', 'bicurious', 'jackoff', 'doggie-style', 'extinguish', 'cumshots', 'nambla', 'wet dream', 'cockmaster', 'assgoblin', 'rapey', 'knobhead', 'ho', 'imbecile', 'pussy', 'reefer', 'bookie', 'disable', 'masturbation', 'bollock', 'kumming', 'wench', 'bunny fucker', 'need the dick', 'cocknugget', 'massa', 'skank', 'polesmoker', 'fuckers', 'hentai', 'molest', 'assbangs', 'cunny', 'shagger', 'shi+', 'assassination', 'fagfucker', 'potty', 'dominatrix', 'muffdiver', 'nobjokey', 'footjob', 'porn', 'affliction', 'crack', 'dopey', 'fuckme', 'pcp', 'shittiest', 'assbandit', 'wound', 'woe', 'queerbait', 'dickbag', 'pussy juice', 'cockeye', 'hemp', 'intact', 'assklown', 'sucks', 'cum chugger', 'spread legs', 'ninny', 'bohunk', 'mr hands', 'footfuck', 'asspacker', 'tyrannize', 'hootch', 'rum', 'peckerhead', 'figging', 'xrated', 'headfuck', 'cuntface', 'fuckings', 'slut bucket', 'upskirt', 'hitler', 'gangbangs', 'lmfao', 'bitching', 'cummer', 'footlicker', 'tittyfuck', 'whores', 'fuckingshitmotherfucker', 'kyke', 'how to kill', 'jelly donut', 'pussys', 'creampie', 'fagots', 'pisser', 'fistfucker', 'mothafucks', 'piss pig', 'opium', 'how to murder', 'humped', 'anus', 'breastman', 'nut butter', 'pimpis', 'fanny', 'asskiss', 'drunk', 'chink', 'fudge packer', 'twatwaffle', 'gspot', 'chocolate rosebuds', 'dummy', 't1tt1e5', 'fxck', 'ovums', 'paedophile', 'cameltoe', 't1tties', 'buggered', 'breasts', 'fuck-bitch', 'asswipes', 'cornhole', 'hot carl', 'arse', 'ovary', 'dick head', 'dildos', 'fagot', 'oral', 'cockknocker', 'dendrophilia', 'doggy style', 'kinkster', 'whorehopper', 'dyke', 'assfucker', 'urinal', 'bullshits', 'fingerfucked', 'dicktickler', 'pissin', 'renob', 'twink', 'hardcoresex', 'cumslut', 'bod', 'stoned', 'darn', 'numskull', 'clam', 'cocksucking', 'scrog', 'teat', 'pubic hair', 'retard', 'barfface', 's-h-1-t', 'cockmunch', 'flog the log', 'queerhole', 'sanger', 'fart', 'assface', 'a54', 'son of a motherless goat', 'fist fuck', 'fool', 'sumofabiatch', 'pissflaps', 'fuck-ass', 'cuntlicker', 'fukwhit', 'gang-bang', 'fuq', 'nazism', 'contaminate', 'psycho', 'herpy', 'dicksucking', 'boners', 'fuckin', 'tw4t', 'doggin', 'rack', 'camboy', 'boozy', 'toke', 'gigolo', 'vibrator', 'gaylord', 'stroke', 'facefucker', 'hard core', 'fuckoff', 'camel toe', 'viagra', 'assfukka', 'cuntslut', 'escort', 'poop', 's hit', 'titwank', 'oppress', 'taig', 'shitfaced', 'bi-sexual', 'lesbo', 'niggas', 'assmonkey', 'blowjobs', 'prod', 'skeet', 'weenie', 'violet wand', 'goo girl', 'chi-chi man', 'cocklump', 'facial', '5hit', 'jiggerboo', 'foah', 'deep throat', 'shagging', 'dickweed', 'asswad', 'gae', 'twatlips', 'shitcanned', 'dickface', 'eatpussy', 'fuck-tard', 'voyeur', 'jizzed', 't1t', 'queer', 'pricks', 'quim', 'banger', 'nymphomania', 'orally', 'bastinado', 'twats', 'cybersex', 'spac', 'goatse', 'tard', 'fuckyou', 'butchery', 'he-she', 'g-spot', 'phuck', 'sexual', 'jugs', 'bung', 'bondage', 'bulldyke', 'naked', 'mothafuckings', 'gangbanged', 'dipshit', 'menage a trois', 'fack', 'intimidate', 'poon', 'tears', 'cocksucker', 'mother fucker', 'hore', 'honkers', 'cocklicker', 'dick', 'hooter', 'suckass', 'fannybandit', 'fondle', 'perversion', 'tub girl', 'peyote', 'b!tch', 'hotpussy', 'tampon', 'sandbar', 'assbanger', 'scat', 'buttman', 'womb', 'poop chute', 'sadism', 'porno', 'menses', 'dirsa', 'mothafuckaz', 'moron', 'trumped', 'carpetmuncher', 'dvda', 'hoar', 'p.u.s.s.y.', 'pasty', 'sodomy', 'bum', 'vulva', 'bastard', 'kraut', 'fuckwad', 'whorebag', 'c0cksucker', 'beaver cleaver', 'choad', 'feck', 'porchmonkey', 'raghead', 'grief', 'dickmilk', 'meth', 'masochist', 'erotic', 'jizz', 'chincs', 'freakyfucker', 'vodka', 'nigg3r', 'sodom', 'cox', 'cuntrag', 'areole', 'bumblefuck', 'wang', 'jagoff', 'gonad', 'panties', 'brotherfucker', 'shittier', 'misery', 'injun', 'labia', 'ugly', 'goddammit', 'transsexual', 'orgasims', 'mongoloid', 'beardedclam', 'shitface', 'cockwaffle', 'jism', 'two fingers with tongue', 'slag', 'dipship', 'piss', 'rimjob', 'gender bender', 'one guy one jar', 'son of a bitch', 'bastardo', 'penisbanger', 'cockshit', 'feltch', 'freakfuck', 'urophilia', 'cumjockey', 'pubes', 'member', 'breast', 'intercourse', 'war', 'dago', 'shits', 'hooch', 'shiz', 'tied up', 'buffoon', 'tea bagging', 'duche', 'dommes', 'testicle', 'wop', 'waste', 'fecal', 'tittyfucker', 'boozer', 'camslut', 'asssucker', 'rump', 'doggystyle', 's.h.i.t.', 'cyalis', 'wee-wee', 'maiden', 'motherfuckers', 'caca', 'fecker', 'jerkoff', 'chaste', 'circlejerk', 'fuck you', 'assjockey', 'gooks', 'sleaze', 'knobjocky', 'call girl', 'dong', 'mtherfucker', 'scroat', 'wiseasses', 'pussy palace', 'cunilingus', 'babe', 'cyberfuck', 'coprolagnia', 'menstruation', 'cocksucer', 'kums', 'how to make a bomb','tubgirl', 'birdlock', 'weirdo', 's_h_i_t', 'brothel', 'asslicker', 'cumguzzler', 'foreskin', 'lezbo', 'fistfuckings', 'pedophiliac', 'butt-fuck', 'dickfucker', 'godsdamn', 'god-dam', 'assnigger', 'xxx', 'homo', 'undies', 'shittings', 'buttface', 'kwif', 'shitfull', 'crappy', 'freaking', 'prickteaser', 's-h-i-t', 'blow me', 'jack-off', 'cuntass', 'twunt', 'arian', 'birth control', 'donkeypunch', 'twat', 'blue waffle', 'puto', 'bootee', 'booooobs', 'fistfucking', 'slaughterhouse', 'c0ck', 'untouched', 'assfuck', 'barface', 'defile', 'autoerotic', 'cripple', 'destroy', 'threat', 'clitface', 'whore', 'brutality', 'futanari', 'nympho', 'pigfucker', 'bully', 'tities', 'topless', 'pisses', 'bugger', 'buttplug', 'scum', 'wreck', 'pussypounder', 'maidenly', 'cockblock', 'handjob', 'omorashi', 'lemon party', 'reetard', 'p0rn', 'pussi', 'condom', 'gash', 'doggiestyle', 'wankjob', 'ghay', 'pain', 'toots', 'gang bang', 'clit', 'b1tch', 'donkeyribber', 'blow your load', 'screwing', 'd1ldo', 'vomit', 'lezzy', 'motherfucking', 'snatch', 'slaughter', 'cnut', 'motherfuckin', 'whitey', 'cockass', 'thug', 'abuse', 'punkass', 'queero', 'c.0.c.k', 'urethra play', 'lezbos', 'freex', 'cunntt', 'bunga', 'debauch', 'busty', 'boiolas', 'homoey', 'fenian', 'clits', 'rosy palm', 'ecchi', 'bitchtits', 'eat hair pie', 'shitblimp', 'old bag', 's-o-b', 'fux', 'd0uch3', 'urine', 'pantie', 'goddamnit', 'knob end', 'kill', 'cocksmith', 'cockhead', '5h1t', 'bestiality', 'hoer', 'motherfuck', 'booze', 'fuckedup', 'terd', 'chinky', 'nad', 'dumb ass', 'endanger', 'doggy-style', 'reich', 'cunnie', 'goddam', 'bust a load', 's.o.b.', 'menstruate', 'balllicker', 'flamer', 'coonnass', 'sexy', 'son of a whore', 'gey', 'poof', 'gayfuckist', 'buggery', 'yobbo', 'zoophile', 'hooker', 'nincompoop', 'double dong', 'buttfucka', 'dumbshit', 'menace', 'choade', 'phalli', 'torment', 'kitty', 'fingerfucker', 'dickjuice', 'jack off', 'vjayjay', 'dog style', 'dogging', 'pube', 'wetback', 'smartass', 'bitchers', 'fingerbang', 'assbanged', 'ginger', 'yellow showers', 'cumdumpster', 'muthafuckaz', 'swastika', 'enlargement', 'kummer', 'schizo', 'gotohell', 'wog', 'mofo', 'ball licking', 'ejaculated', 'crack-whore', 'fags', 'genitals', 'violence', 'taff', 'fcuker', 'trashy', 'god-damned', 'blonde on blonde action', 'minger', 'crackwhore', 'punky', 'fukkers', 'clitfuck', 'hotsex', 'shamedame', 'a2m', 'lezza', 'sultry women', 'thrust', 'slave', 'nig nog', 'fuckingbitch', 'sod', 'faggots', 'areola', 'racy', 'beotch', 'a_s_s', 'asspirate', 'f_u_c_k', 'dry hump', 'iap', 'cocks', 'mo-fo', 'teabagging', 'homey', 'mcfagget', 'douchey', 'barf', 'cunnilingus', 'fucknut', 'penetrate', 'big knockers', 'lap dance', 'fuck puppet', 'harm', 'masterbation', 'destruction', 'a$$hole', 'fook', 'motherfucka', 'shitey', 'fistfucked', 'japs', 'booty call', 'flaps', 'ma5terb8', 'nigaboo', 'kkk', 'spotless', 'panooch', 'clusterfuck', 'jeopardize', 'a$$', 'nawashi', 'jailbait', 'gaydo', 'shitbrains', 'double penetration', 'asspuppies', 'axwound', 'muthrfucking', 'eunuch', 'mafugly', 'screwed', 'pussy fart', 'bullturds', 'fuck off', 'midget', 'cock snot', 'bimbo', 'scrud', 'bullcrap', 'gays', 'hand job', 'girl on', 'dumbbell', 'buttfucker', 'dickfuck', 'gaywad', 'knobead', 'chastity', 'cum freak', 'bitchy', 'boner', 'mutilate', 'coochy', 'pussylicking', 'tittywank', 'bitchass', 'phonesex', 'cuntbag', 'fucks', 'black cock', 'dickzipper', 'cumtart', 'bitches', 'alaskan pipeline', 'yid', 'shibari', 'nitwit', 'testee', 'm45terbate', 'chin', 'honky', 'shithouse', 'assh0le', 'cocksuka', 'queaf', 'veqtable', 'crotte', 'junglebunny', 'boooobs', 'butt', 'dickish', 'souse', 'butt-fucker', 'boobs', 'gonads', 'testical', 'willy', 'fucka', 'hooters', 'dumbcunt', 'carpet muncher', 'pisspig', 's&m', 'sandnigger', 'tosser', 'dick-sneeze', 'flange', 'dicks', 'shitters', 'pthc', 'slutkiss', 'slutbag', 'hardcore', 'eradicate', 'godamn', 'slut', 'm0f0', 'penile', 'fuckup', 'uterus', 'barely legal', 'tawdry', 'melons', 'jerk off', 'n1gger', 'cl1t', 'virginality', 'dick shy', 'taking the piss', 'bollox', 'shiting', '4r5e', 'shitass', 'niglet', 'pornography', 'gayass', 'pron', 'sandler', 'fuck yo mama', 'kawk', 'hussy', 'ganja', 'cockknoker', 'cumstain', 'fingerfucks', 'dickwod', 'felch', 'rimjaw', 'yiffy', 'hump', 'teste', 'dumbfuck', 'fuckass', 'assmunch', 'pussies', 'cawk', 'bullet vibe', 'pissoff', 'cunts', 'cockfucker', 'n1gga', 'despair', 'ruski', 'ball sucking', 'horny', 'tribadism', 'v14gra', 'chinc', 'fagbag', 'window licker', 'munging', 'orgasmic', 'twatty', 'penis', 'stfu', 'tits', 'fuckwitt', 'shemale', 'mthrfucker', 'hun', 'sadist', 'goddamnmuthafucker', 'undressing', 'ghey', 'ball gravy', 'azazel', 'cocksniffer', 'phuk', 'buttmuch', 'dumshit', 'glans', 'fagtard', 'cyberfucked', 'demolish', 'spooge', 'lusty', 'f u c k', 'piece of shit', 'cock-sucker', 'anilingus', 'homicide', 'shitheads', 'fucktart', 'weiner', 'beaner', 'c.u.n.t', 'j3rk0ff', 'murder', 'mothafucking', 'scrot', 'tit', 'slanteye', 'shitcunt', 'ass fuck', 'c-0-c-k', 'vulgar', 'erection', 'scrote', 'daterape', 'clitty', 'pillowbiter', 'cunt hair', 'napalm', 'fatfucker', 'a55', 'blockhead', 'fuckbag', 'beer', 'baby batter', 'fukker', 'faggs', 'bdsm', 'bollocks', 'corp whore', 'dunce', 'niggaz', 'slutdumper', 'exploit', 'virtue', 'bitched', 'master-bate', 'bestial', 'fuckbrain', 'muther', 'badfuck', 'leather straight jacket', 'assranger', 'bitchez', 'big breasts', 'femdom', 'obliterate', 'gangbang', 'bloody hell', 'prick', 'jackasses', 'poopuncher', 'wiseass', 'nigger', 'jerk-off', 'rectum', 'punanny', 'blonde action', 'dykes', 'fuckwit', 'tittie5', 'assman', 'rectus', 'nimphomania', 'cocknose', 'bowel', 'poontang', 'godamnit', 'jail bait', 'asslick', 'fagg', 's0b', 'style doggy', 'erotism', 'dickbeaters', 'analannie', 'suck', '69', 'ruin', 'golliwog', 'soused', 'cockmongruel', 'skag', 'douche', 'goodpoop', 'phone sex', 'dicksipper', 'auto erotic', 'fisting']

attempt_count = 0

file_path = './keylogs.txt'

# Get absolute path
abs_file_path = os.path.abspath(file_path)
# Function to handle key presses
current_word = ""

dt = datetime.datetime.now()
dt_str = dt.strftime("%Y-%m-%d %H:%M:%S")

device_name = socket.gethostname()

print(file_path)
def show_warning():
    root = tk.Tk()
    root.withdraw()  # Hide the root window
    root.attributes("-topmost", True)  # Ensure the dialog appears on top
    root.grab_set()  # Grab the focus
    messagebox.showwarning("Warning", "You have entered banned words in your browser\nAs a result, your access to this service has been restricted.")
    root.mainloop()

def show_alert():
    root = tk.Tk()
    root.withdraw()  # Hide the root window
    root.attributes("-topmost", True)  # Ensure the dialog appears on top
    root.grab_set()  # Grab the focus
    messagebox.showinfo("Alert", "Your PC will be shut down if you continue to search for inappropriate content.")
    root.mainloop()

def show_shut():
    root = tk.Tk()
    root.withdraw()  # Hide the root window
    root.attributes("-topmost", True)  # Ensure the dialog appears on top
    root.grab_set()  # Grab the focus
    messagebox.showerror("Shutdown Alert", "Your system is going to shut down!")
    root.mainloop()

def on_press(key):
    global current_word
    # Log only the keystrokes
    if hasattr(key, 'char'):
        char = key.char
        current_word += char
        # Save the keystroke
        # logging.info(char)
    elif key == Key.space  or key == Key.tab:
        current_word+=" "
        # Check the word when space, enter, or tab is pressed
    elif key == Key.enter :
        logging.info(current_word)
        check_word(current_word.strip())
        current_word = ""  # Reset current word
    elif key == Key.backspace:
        # Handle backspace
        current_word = current_word[:-1]


# Function to check the word against the list of banned words
def close_br():
    global attempt_count
    windows = gw.getAllWindows()
    attempt_count += 1
    for window in windows:
        if "chrome" in window.title.lower() or "edge" in window.title.lower() or "firefox" in window.title.lower() or "brave" in window.title.lower() or "tor" in window.title.lower() or "brave" in window.title.lower():
            # Close the window if it's a browser window
            window.close()
    # Display appropriate message and take action based on attempt count
    if attempt_count == 1:
         #title = "Warning: Banned Words Detected"
         # message = "\t\tYou have entered banned words in your browser\n\nAs a result, your access to this service has been restricted.\nPlease refrain from using inappropriate language to avoid further action."
         threading.Thread(target=show_warning).start()
         #d_box(attempt_count)'

    elif attempt_count == 2:
        # title = "ALERT"
        # message = "\tYour PC will be shut down if you continue to search for inappropriate content."
        threading.Thread(target=show_alert).start()
        #d_box(attempt_count)

    elif attempt_count==3:
        threading.Thread(target=show_shut).start()
        email_alert("ALERT: Inappropriate Online Activity Detected",
                    "\n\tWe regret to inform you that we have detected potentially concerning online activity involving your child. Our monitoring system has flagged inappropriate content being accessed or searched for.\n\nDetails:\n\nDate and Time: {}\nDevice name:{}\nActivity: This device has searched for the phrase {} in the browser, indicating potential engagement with inappropriate"
                    " content.".format(dt_str,device_name, current_word), "geerthi006@gmail.com", file_path)
        os.system("shutdown /s /t 1")

def check_word(word):
    global attempt_count
    windows = gw.getAllWindows()

    for banned_word in banned_words:
        if banned_word in word.lower():
                close_br()





# Start listening for keystrokes
with Listener(on_press=on_press) as listener:
    try:
        listener.join()  # Start the listener
    except KeyboardInterrupt:
        print("Keylogger stopped by user.")
        listener.stop()


```