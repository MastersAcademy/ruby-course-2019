### TOPIC: Objects
### TASK #1 REQUIRED
There is a class MarioGame (mario_game.rb).
This class contains methods, which change input parameters (for example, `change_level`, `change_background`). You have to add to this class additional methods, which print instance variables of this class.
For example, there is a method `show_count_of_enemies`, which prints a value of the variable `@enemies`
Implement similar methods for other variables.
The task is to implement your new methods in the separated module and connect it to the class MarioGame.
In the end, you should have something like this:
```ruby
class MarioGame
  include SomeModule
end
```
2. Create  a class for some another game with some another name. Your game should contain its own methods. Methods should be implemented in the module, which you included into `MarioGame`. DO NOT USE INHERITANCE!
### TASK #2 REQUIRED
We have implemented a "Mario" game, based on the class `MarioGame`. Each level is a new instance of this class.  
For example, the game has 20 levels and instances are declared in the next way:
```ruby
mario1 = MarioGame.new
mario2 = MarioGame.new
.......................
mario20 = MarioGame.new
```
On the last level we should have a method, which prints a final splash screen, which is not available on other levels.
### TASK #3 *
Implement a class, which selects all even numbers from the given array. This array can contain numbers (1, 2, 3) or stringfied numbers ('1', '123', '123.5', '10')
### TASK #4 *
Implement a class for math operations. Methods of this class should be declared dynamically.
### TASK #5 *
Implement a class, which prints given text with a given color. Methods of this class should be declared dynamically. DO NOT use additional libraries.
Input example: `ruby task5.rb 'my awesome text' red`
### TASK #6 *
Implement a class for math operations, result of each operation should be printed with its own color. Usage of mixins is required. DO NOT use additional libraries.
### TASK #7 *
Implement a class, which checks syntax of open/closed braces in a given string. For example:
`"{aaa}" - valid`
`"{aaa" - invalid`
`"{(aaa}" - invalid`
`"{(aaa})" - invalid`
### TASK #8 ***
Implement a class, that implements a "Mario" game through the console stdin/stdout. For example, you have a start picture:
```
                                                                              ####
                                                                            ########
      ##                                                                  ############
     ####                                                               ################
      ##                                                              ####################
#################################################################################################
```
When I click a key on the keyboard, a character moves:
```
                                                                              ####
                                                                            ########
                 ##                                                       ############
                ####                                                    ################
                 ##                                                   ####################
############################################################################################################
```
```
                                                                              ####
                                                                            ########
                       ##                                                 ############
                      ####                                              ################
                       ##                                             ####################
##################################################################################################################
```
When a character reaches a pyramid, a flag is raised on the pyramid
```
                                                                             ####
                                                                             ####
                                                                                #
                                                                              ####
                                                                            ########
                                                               ##         ############
                                                              ####      ################
                                                               ##     ####################
##################################################################################################################

```
