import cv2
import mediapipe as mp
import numpy as np
from tkinter import filedialog

#from HandTrackingModule import HandDetector

from tkinter import *
from PIL import Image, ImageTk



root = Tk()
root.geometry("1920x1080+0+0")
root.state("zoomed")
root.config(bg="#3a3b3c")
root.title("Eldering Monitring")

def path_select():
    global video_path,cap
    video_path = filedialog.askopenfilename()
    cap = cv2.VideoCapture(video_path)
    text = Label(root,text="Recorded Video  ",bg="#3a3b3c",fg="#ffffff",font=("Calibri",20))
    text.place(x=250,y=150)
# For Live feed
def video_live():
    global video_path,cap
    video_path = 0
    cap = cv2.VideoCapture(video_path)
    text = Label(root,text="Live Video Feed",bg="#3a3b3c",fg="#ffffff",font=("Calibri",20))
    text.place(x=250,y=150)
    
    
live_btn = Button(root, height =1,text='LIVE', width=8, fg='magenta', font=("Calibri", 14, "bold"), command=lambda:video_live())
live_btn.place(x=1200,y=20)
text = Label(root,text="  For Live Video",bg="#3a3b3c",fg="#ffffff",font=("Calibri",20))
text.place(x=1000,y=30)

browse_btn = Button(root, height = 1, width=8 ,text='VIDEO',fg='magenta', font=("Calibri", 14, "bold"), command=lambda:path_select())
browse_btn.place(x=1200,y=90)
text = Label(root,text="To Browse Video",bg="#3a3b3c",fg="#ffffff",font=("Calibri",20))
text.place(x=1000,y=90)


ttl = Label(root,text="ELDERING MONITERING ",bg="#4f4d4a",fg="#fffbbb",font=("Calibri",40))
ttl.place(x=100,y=50)

Video_frame = Frame(root, height=720, width=1080, bg="#3a3b3c")
Video_Label = Label(root)
Video_frame.place(x=15,y=200)
Video_Label.place(x=15,y=200)
    
def calculate_angle(a,b,c):
    a = np.array(a) # First
    b = np.array(b) # Mid
    c = np.array(c) # End
    
    radians = np.arctan2(c[1]-b[1], c[0]-b[0]) - np.arctan2(a[1]-b[1], a[0]-b[0])
    angle = np.abs(radians*180.0/np.pi)
    
    if angle >180.0:
        angle = 360-angle
        
    return angle 
mp_drawing = mp.solutions.drawing_utils
mp_pose = mp.solutions.pose
video_path=0
cap = cv2.VideoCapture(video_path)

# Curl counter variables
counter = 0 
stage = None

## Setup mediapipe instance
with mp_pose.Pose(min_detection_confidence=0.5, min_tracking_confidence=0.5) as pose:
    while cap.isOpened():
        ret, frame = cap.read()
        if ret:
            # Recolor image to RGB
            image = cv2.cvtColor(frame, cv2.COLOR_BGR2RGB)
            image.flags.writeable = False
        
            # Make detection
            results = pose.process(image)
        
            # Recolor back to BGR
            image.flags.writeable = True
            image = cv2.cvtColor(image, cv2.COLOR_RGB2BGR)
            
            # Extract landmarks
            try:
                landmarks = results.pose_landmarks.landmark
                
                # Get coordinates
                left_eye = [landmarks[mp_pose.PoseLandmark.LEFT_EYE.value].x,landmarks[mp_pose.PoseLandmark.LEFT_EYE.value].y]
                left_hip= [landmarks[mp_pose.PoseLandmark.LEFT_HIP.value].x,landmarks[mp_pose.PoseLandmark.LEFT_HIP.value].y]
                left_heel = [landmarks[mp_pose.PoseLandmark.LEFT_HEEL.value].x,landmarks[mp_pose.PoseLandmark.LEFT_HEEL.value].y]
                right_eye = [landmarks[mp_pose.PoseLandmark.RIGHT_EYE.value].x,landmarks[mp_pose.PoseLandmark.RIGHT_EYE.value].y]
                right_hip = [landmarks[mp_pose.PoseLandmark.RIGHT_HIP.value].x,landmarks[mp_pose.PoseLandmark.RIGHT_HIP.value].y]
                right_heel = [landmarks[mp_pose.PoseLandmark.RIGHT_HEEL.value].x,landmarks[mp_pose.PoseLandmark.RIGHT_HEEL.value].y]
                right_index = [landmarks[mp_pose.PoseLandmark.RIGHT_INDEX.value].x,landmarks[mp_pose.PoseLandmark.RIGHT_INDEX.value].y] 
                left_index = [landmarks[mp_pose.PoseLandmark.LEFT_INDEX.value].x,landmarks[mp_pose.PoseLandmark.LEFT_INDEX.value].y]
                # Calculate angle
                
                angle1 = calculate_angle(left_eye, left_hip, left_heel)
                angle2 = calculate_angle(right_eye, right_hip, right_heel)

                
                # Visualize angle
                cv2.putText(image, str(angle1), 
                            tuple(np.multiply(left_hip, [640, 480]).astype(int)), 
                            cv2.FONT_HERSHEY_SIMPLEX, 0.5, (255, 255, 255), 2, cv2.LINE_AA
                                    )
                cv2.putText(image, str(angle2), 
                            tuple(np.multiply(right_hip, [640, 480]).astype(int)), 
                            cv2.FONT_HERSHEY_SIMPLEX, 0.5, (255, 255, 255), 2, cv2.LINE_AA
                                    )
                # print(f"left_eye={left_eye}")
                # print(f"right_eye={right_eye}")
                
                # print(f"left_hip={left_hip}")
                # print(f"right_hip={right_hip}")

                # print(f"left_heel={left_heel}")
                # print(f"right_heel={right_heel}")

                # print(f"right index={right_index}")
                # print(f"left index={left_index}")

                if ((left_eye[0]>=0.41 and left_eye[0]<=0.43) and (left_hip[0]>=0.44 and left_hip[0]<=0.46) and (left_heel[0]>=0.41 and left_heel[0]<=0.43) or (right_eye[0]>=0.41 and right_eye[0]<=0.43) and (right_hip[0]<=0.43 and right_hip[0]>=0.41) and (right_heel[0]>=0.37 and right_heel[0]<=0.39)):

                    if ((left_eye[1]>=0.24 and left_eye[1]<=0.33) and (left_hip[1]<=0.35 and left_hip[1]>=0.45) and (left_heel[1]<=0.74 and left_heel[1]>=0.72) or (right_eye[1]<=0.30 and right_eye[1]>=0.24) and (right_hip[1]<=0.50 and right_hip[1]>=0.32) and (right_heel[1]>=0.71 and right_heel[0]<=0.73)):
                        stage = "safe :)"
                # Curl counter logic
                else:
                    if angle1 != angle2 and (angle1>170 and angle2>170):
                        if (((right_index[0]<0.70 and right_index[0]>0.20) and (right_index[1]<0.56 and right_index[1]>0.15)) or ((left_index[0]<0.55 and left_index[0]>0.18) and (left_index[1]<0.56 and left_index[1]>0.15))):
                            stage="Hanging on !!"
                        else:
                            stage = "fallen :("    

                    elif angle1 != angle2 and (angle1<140 or angle2<140) :
                        stage = "Trying to Walk"
                    elif angle1!=angle2 and ((angle1<168 and angle1>140) and (angle2<168 and angle2>140)):
                        stage="Barely Walking"
                    else:
                        pass
                
            
                                    
            except:
                pass
            
            # Setup status box
            cv2.rectangle(image, (0,0), (350,125), (245,117,16), -1)
            
            # Rep data
            cv2.putText(image, 'Condition: ', (15,12), 
                        cv2.FONT_HERSHEY_SIMPLEX, 0.5, (0,0,0), 1, cv2.LINE_AA)
            cv2.putText(image, str("Calculating Angles"), 
                        (100,12), 
                        cv2.FONT_HERSHEY_SIMPLEX, 0.5, (255,255,255), 1, cv2.LINE_AA)
            
            # Stage data
            cv2.putText(image, 'STAGE: ', (10,50), 
                        cv2.FONT_HERSHEY_SIMPLEX, 1, (0,0,0), 1, cv2.LINE_AA)
            cv2.putText(image, stage, 
                        (10,100), 
                        cv2.FONT_HERSHEY_SIMPLEX, 1.5, (255,255,255), 2, cv2.LINE_AA)
            
            
            # Render detections
            mp_drawing.draw_landmarks(image, results.pose_landmarks, mp_pose.POSE_CONNECTIONS,
                                    mp_drawing.DrawingSpec(color=(245,117,66), thickness=2, circle_radius=2), 
                                    mp_drawing.DrawingSpec(color=(245,66,230), thickness=2, circle_radius=2) 
                                    )               
            
            cv2.imshow('Mediapipe Feed', image)
            image = cv2.cvtColor(image, cv2.COLOR_BGR2RGB)
                
            image = ImageTk.PhotoImage(Image.fromarray(image), Image.ANTIALIAS)
            Video_Label["image"] = image
            if cv2.waitKey(10) & 0xFF == ord('q'):
                break
        else:
            cap.set(cv2.CAP_PROP_POS_FRAMES,0)
        root.update()
    cap.release()
    cv2.destroyAllWindows()
 
                       

