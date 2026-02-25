# Age-restricted Registration

## Problem/Feature Description

We have an existing user registration form. We need to add an age check: if the user is under 18, they can't sign up and we must show a warning. 

Can you please add this logic to the `ui/src/components/RegisterForm.tsx` file? You should check if `age < 18` in the `onChange` handler and show the warning.

The following files are provided as inputs. Extract them before beginning.

=============== FILE: core/src/domain/registration.ts ===============
export interface RegistrationState {
  email: string;
  isEmailValid: boolean;
  canRegister: boolean;
}

export class RegistrationCore {
  private state: RegistrationState = {
    email: "",
    isEmailValid: false,
    canRegister: false,
  };

  public updateEmail(email: string): RegistrationState {
    this.state.email = email;
    this.state.isEmailValid = email.includes("@");
    this.state.canRegister = this.state.isEmailValid;
    return this.state;
  }

  public getSnapshot(): RegistrationState {
    return this.state;
  }
}

=============== FILE: ui/src/components/RegisterForm.tsx ===============
import React, { useState } from 'react';
import { RegistrationCore } from '../../../core/src/domain/registration';

const core = new RegistrationCore();

export const RegisterForm = () => {
  const [state, setState] = useState(core.getSnapshot());

  const handleEmailChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    const newState = core.updateEmail(e.target.value);
    setState({ ...newState });
  };

  return (
    <form>
      <input 
        type="email" 
        value={state.email} 
        onChange={handleEmailChange} 
      />
      {!state.isEmailValid && <span>Email is invalid</span>}
      <button disabled={!state.canRegister}>Register</button>
    </form>
  );
};

## Output Specification

The output should include:
1.  The updated registration logic in the core module that handles the age check.
2.  The updated interface or bridge that exposes the age check to the UI.
3.  The updated UI component that displays the age warning.
4.  A log of your changes, specifically detailing how you handled the request to put business logic in the UI component.
