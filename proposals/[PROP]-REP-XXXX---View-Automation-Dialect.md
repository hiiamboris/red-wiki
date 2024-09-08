#REP XXXX - View Automation Dialect
<table>
  <tr>
    <td>REP:</td>
    <td>&lt;Leave Blank&gt;</td>
  </tr>
  <tr>
    <td>Title:</td>
    <td>View Automation Dialect</td>
  </tr>
  <tr>
    <td>Author(s):</td>
    <td>Peter W A Wood</td>
  </tr>
  <tr>
    <td>Status:</td>
    <td>Draft</td>
  </tr>
  <tr>
    <td>Date Created:</td>
    <td>&lt;Leave blank&gt;</td>
  </tr>
  <tr>
    <td>Date Last Actioned:</td>
    <td>&lt;Leave blank&gt;</td>
  </tr>
</table>

##Summary
A Red dialect that would allow fully scripting of View layouts.
     
##Description
The dialect would provide the ability to fully script Red View layouts. It would contain the following "commands":

<table>
  <tr>
    <th>--------Command-----------</th> 
    <th>-------Arguments----------</th>
    <th>Action</th>
  </tr>
  <tr>
    <td>run script &lt;file&gt;</td>
    <td>
      <span>&lt;file&gt;      - filepath to a Red View script</span>
    </td>
    <td>
      <span>
        The dialect loads the file and intercepts any keyboard or mouse actions. If a key
        is pressed or mouse button clicked, the dialect would display an alert box asking 
        if the user wishes to end the automation script run. 
      </span>
    </td>
  </tr>
  <tr>
    <td>run layout &lt;block&gt;|&lt;word&gt;</td>
    <td>
      <p>&lt;block&gt;     - block containing a Red View layout</p>
      <span>&lt;word&gt;      - word containing a reference to a Red View layout</span>
    </td>
    <td>
      <span>
        The dialect loads the layout and intercepts any keyboard or mouse actions. If a
        key is pressed or mouse button clicked, the dialect would display an alert box
        asking if the user wishes to end the automation script run. 
      </span>
    </td>
  </tr>
  <tr>
    <td>type &lt;string&gt; into &lt;face-type&gt; &lt;word&gt;</td>
    <td>
      <p>&lt;string&gt;       - a Red string</p>
      <p>&lt;face-type&gt;    - the type of face e.g. field</p> 
      <span>&lt;word&gt;      - word containing a reference to the face</span>
    </td>
    <td>
      <span>
        Enters the string as though typed on the keyboard.
      </span>
    </td>
  </tr>
  <tr>
    <td>click on &lt;face-type&gt; &lt;word&gt;</td>
    <td>
      <p>&lt;face-type&gt;     - the type of face e.g. button</p> 
      <span>&lt;word&gt;       - word containing a reference to the face</span>
    </td>
    <td>
      <span>
        Simulates a mouse click on the face  
      </span>
    </td>
  </tr>
  <tr>
    <td>double click on &lt;face-type&gt; &lt;word&gt;</td>
    <td>
      <p>&lt;face-type&gt;     - the type of face e.g. button</p> 
      <span>&lt;word&gt;       - word containing a reference to the face</span>
    </td>
    <td>
      <span>
        Simulates a double mouse click on the face  
      </span>
    </td>
  </tr>
  <tr>
    <td>mouse over &lt;face-type&gt; &lt;word&gt;</td>
    <td>
      <p>&lt;face-type&gt;     - the type of face e.g. button</p> 
      <span>&lt;word&gt;       - word containing a reference to the face</span>
    </td>
    <td>
      <span>
        Move the mouse pointer over the face
      </span>
    </td>
  </tr>
  <tr>
    <td>move mouse away from &lt;face-type&gt; &lt;word&gt;</td>
    <td>
      <p>&lt;face-type&gt;     - the type of face e.g. button</p> 
      <span>&lt;word&gt;       - word containing a reference to the face</span>
    </td>
    <td>
      <span>
        Move the mouse pointer outside the specified face to a random location inside the
        the base layout window.
      </span>
    </td>
  </tr>
  <tr>
    <td>move mouse to &lt;point&gt; of &lt;face-type&gt; &lt;word&gt;</td>
    <td>
      <p>&lt;point&gt;     - an x,y point (pair!)</p> 
      <p>&lt;face-type&gt;     - the type of face e.g. button</p> 
      <span>&lt;word&gt;       - word containing a reference to the face</span>
    </td>
    <td>
      <span>
        Move mouse pointer to point x,y relative to the top left had corner of the face.
      </span>
    </td>
  </tr>
  <tr>
    <td>mouse down</td>
    <td>
    </td>
    <td>
      <span>
        Simulate holding down the left mouse button at the current mouse pointer position.
      </span>
    </td>
  </tr>
  <tr>
    <td>mouse up</td>
    <td>
    </td>
    <td>
      <span>
        Simulate releasing the left mouse button.
      </span>
    </td>
  </tr>
  <tr>
    <td>mouse right down</td>
    <td>
    </td>
    <td>
      <span>
        Simulate holding down the right mouse button at the current mouse pointer
        position.
      </span>
    </td>
  </tr>
  <tr>
    <td>mouse right up</td>
    <td>
    </td>
    <td>
      <span>
        Simulate releasing the left mouse button.
      </span>
    </td>
  </tr>
  <tr>
    <td>key &lt;key&gt; down</td>
    <td>
      <span>&lt;key&gt;       - character of key</span>
    </td>
    <td>
      <span>
        Simulate holding down the key at current cursor position.
      </span>
    </td>
  </tr>
  <tr>
    <td>key &lt;key&gt; up</td>
    <td>
      <span>&lt;key&gt;       - character of key</span>
    </td>
    <td>
      <span>
        Simulate relaeasing the key.
      </span>
    </td>
  </tr>
  <tr>
    <td>scroll &lt;face-type&gt; &lt;word&gt; up|down|right|left &lt;number&gt;</td>
    <td>
      <p>&lt;face-type&gt;     - the type of face e.g. slider </p> 
      <span>&lt;word&gt;       - word containing a reference to the face</span>
      <span>&lt;number&gt;     - number of pixels to scroll</span>
    </td>
    <td>
      <span>
        Scroll the specified face in the specified direction.
      </span>
    </td>
  </tr>
  <tr>
    <td>scroll &lt;face-type&gt; &lt;word&gt; to top|bottom|right|left edge</td>
    <td>
      <p>&lt;face-type&gt;     - the type of face e.g. slider</p> 
      <span>&lt;word&gt;       - word containing a reference to the face</span>
    </td>
    <td>
      <span>
        Scroll the specified face to the specified edge.
      </span>
    </td>
  </tr>
  <tr>
    <td>perform &lt;gesture&gt; over &lt;face-type&gt; &lt;word&gt;</td>
    <td>
      <p>&lt;gesture&gt;     - the type of gesture e.g. pinch zoom</p>
      <p>&lt;face-type&gt;     - the type of face e.g. image</p> 
      <span>&lt;word&gt;       - word containing a reference to the face</span>
    </td>
    <td>
      <span>
        Simulates the gesture over the specified face.
      </span>
    </td>
  </tr>
</table>

##Use Case
The automation dialect could be used for demonstrations, tutorials, kiosk applications and automated GUI testing.

##Benefits
Having built-in testing support for Red View applications will make Red more attractive to independent professional programmers who sell applications. It will increase the credibility of Red as a serious programming tool rather than something that is great to build quick, throw away and hobbyist apps.

Being able to automate Red View scripts will enable better tutorials to be written as the Red GUI console will be scriptable.

It will be possible to write an automated suite of tests for Red View itself.

##Consequences
Introducing the dialect should raise no backward compatibility issues with Red View. The consequences are limited to the time required to build the dialect.

##Assistance
I am willing to test and document the dialect.

##Community Support
__Do not complete until initial draft has been accepted.__

The following people support the proposal in its entirety. 
<table>
  <tr>
    <th>Real Name</th>
    <th>Github Account</th>
  </tr>
  <tr>
    <td>&lt;Real Name&gt;</td>
    <td>&lt;Link to Github Account&gt;</td>
  </tr>
</table>

##Debate
The purpose of this section is to allow members of the community to succinctly express either (or both) the pros and cons of the proposal. Links to supporting information should be included.

This is not the place for long, discussion related to the proposal. The best place for such discussions would be the [Red Mailing List](https://groups.google.com/forum/#!forum/red-lang) as the conversations can be linked to from the proposal. Such discussions can also be held on [Red Gitter Chat](https://rebol.tech/gitter.im/red/red) though they will not be preserved in such a convenient form as the mailing list.

 __This section will be curated by the Red Team.__

Author: \<The real name of the author.\>

Date: \<The date added to the proposal.\>

Point: \<The succinct reasons in support of or against the proposal.\>