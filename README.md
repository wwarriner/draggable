# draggable

[![View draggable on File Exchange](https://www.mathworks.com/matlabcentral/images/matlab-file-exchange.svg)](https://www.mathworks.com/matlabcentral/fileexchange/78739-draggable)

A modern enhancement of `draggable.m` written by François Buffard. The design is a decorator for other graphics objects, making them draggable. The decorated objects behave transparently like the underlying objects, while adding functionality and properties associated with draggability. The original reason for this version was to provide compatibility with both the old Figure and Axes objects, as well as the new UIFigure and UIAxes objects. From there, it blossomed into a few bug-fixes for unusual corner-case behaviors and a couple of feature enhancements. The original may be found at the [MATLAB FileExchange page](https://www.mathworks.com/matlabcentral/fileexchange/4179-draggabl).

A few features and notes are listed below. Complete backwards compatibility with the original function calls should be be available with one exception. Turning the draggable functionality off and restoring the original functionality of the graphics objects requires deletion of the draggable decorator. It is no longer possible to use `draggable(h, 'off')`.

The justification for using a decorator design is to have the draggable behave like the original graphics object, without the need for storing app data in parent objects, which is potentially fragile if other objects modify the parents. It keeps private information private, and in my opinion provides a cleaner, more encapsulated design.

## New Features:

- **Choice of mouse button**: Users can make an object draggable only by a specific mouse button (left, middle, right), or by all buttons. The default is previous behavior, i.e. all buttons. To use a different combination of buttons, create multiple objects.
- **`on_click_callback`**: Because `on_move_callback` is called frequently this function was added so that expensive singular operations are called only once when the object is first clicked to initiate dragging.
- **Clearer interface for constraints**: slope, x-axis limits (`xlim`) and y-axis limits (`ylim`) are delineated independently as settable properties.
- **Backward compatibility**: No change should be required to the initial call. Other new features are not accessible this way.   To use the new features, assign values to the appropriate properties. It is no longer possible to use the syntax `draggable(h, 'off')`. To turn dragging off call `delete(draggable_object)`.
- **Behaves transparently** like the underlying graphics object for properties not named in this class.

## Important Notes:

- `movefcn` has been renamed `on_move_callback` (`movefcn` may still be used in the constructor)
- `endfcn` has been renamed `on_release_callback` (`endfcn` may still be used in the constructor)
- Internal data is stored entirely within the class, avoiding use of `set/getappdata()`. The caller is responsible for storing and managing created draggable objects.
- Input `"off"` has been removed and that functionality moved to the object destructor. To restore the graphics object's original behavior delete the draggable object using `delete(draggable_object)`.

**WARNING!** To maintain portability between `Figure` and `UIFigure` objects, do not use `gca()` and `gcf()` in callbacks.