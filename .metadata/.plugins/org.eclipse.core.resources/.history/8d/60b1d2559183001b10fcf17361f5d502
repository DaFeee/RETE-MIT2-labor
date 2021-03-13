package hu.bme.mit.yakindu.analysis.workhere;

import java.io.IOException;
import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

import org.eclipse.emf.common.util.TreeIterator;
import org.eclipse.emf.ecore.EObject;
import org.junit.Test;
import org.yakindu.sct.model.sgraph.State;
import org.yakindu.sct.model.sgraph.Statechart;
import org.yakindu.sct.model.sgraph.Transition;
import org.yakindu.sct.model.stext.stext.EventDefinition;
import org.yakindu.sct.model.stext.stext.VariableDefinition;

import hu.bme.mit.model2gml.Model2GML;
import hu.bme.mit.yakindu.analysis.RuntimeService;
import hu.bme.mit.yakindu.analysis.TimerService;
import hu.bme.mit.yakindu.analysis.example.ExampleStatemachine;
import hu.bme.mit.yakindu.analysis.example.IExampleStatemachine;
import hu.bme.mit.yakindu.analysis.modelmanager.ModelManager;

public class Main {
	@Test
	public void test() {
		main(new String[0]);
	}
	
	public static void main(String[] args) {
		ModelManager manager = new ModelManager();
		Model2GML model2gml = new Model2GML();
		
		// Loading model
		EObject root = manager.loadModel("model_input/example.sct");
		
		// Reading model
		Statechart s = (Statechart) root;
		TreeIterator<EObject> iterator = s.eAllContents();

		List<String> valtozok = new ArrayList<>();
		List<String> eventek = new ArrayList<>();
		
		while (iterator.hasNext()) {
			EObject content = iterator.next();
			if(content instanceof VariableDefinition) {
				VariableDefinition var = (VariableDefinition) content;
				valtozok.add(var.getName());
			}
			if(content instanceof EventDefinition) {
				EventDefinition event = (EventDefinition) content;
				eventek.add(event.getName());
			}
		}
		
		
		System.out.print("public class Main {\r\n" + 
				"	\r\n" + 
				"	public static void main(String[] args) throws IOException {\r\n" + 
				"		ExampleStatemachine s = new ExampleStatemachine();\r\n" + 
				"		s.setTimer(new TimerService());\r\n" + 
				"		RuntimeService.getInstance().registerStatemachine(s, 200);\r\n" + 
				"		s.init();\r\n" + 
				"		s.enter();\r\n" + 
				"		s.runCycle();\r\n" + 
				"		print(s);\r\n" + 
				"		Scanner scanner = new Scanner(System.in);\r\n" + 
				"		String event = scanner.next();\r\n" + 
				"		while (!event.equals(\"exit\")) {\r\n" + 
				"			switch(event) {\r\n" );
		
		for(int i = 0;i<eventek.size();i++) {
			System.out.print("			case \"" + eventek.get(i) + "\":\r\n" + 
					"				s.raise" + eventek.get(i) + "();\r\n" + 
					"				s.runCycle();\r\n" + 
					"				break;\r\n");
		}
		
		System.out.print("			}\r\n" + 
				"			print(s);\r\n" + 
				"			event = scanner.next();\r\n" + 
				"		}\r\n" + 
				"		System.exit(0);\r\n" + 
				"	}\r\n");
		
		System.out.print("	public static void print(IExampleStatemachine s) {\r\n");
		
		for(int i = 0;i<valtozok.size();i++) {
			System.out.print("		System.out.println(\"" + valtozok.get(i) + " = \" + s.getSCInterface().get"+ valtozok.get(i)+ "());\r\n");
		}
		
		System.out.print("	}\r\n" + 
				"}");
		

		
		/*while (iterator.hasNext()) {
			EObject content = iterator.next();
			if(content instanceof VariableDefinition) {
				VariableDefinition var = (VariableDefinition) content;
				System.out.println(var.getName());
			}
			if(content instanceof EventDefinition) {
				EventDefinition event = (EventDefinition) content;
				System.out.println(event.getName());
			}
		}*/

				
		/*int nev = 0;
		while (iterator.hasNext()) {
			EObject content = iterator.next();
			if(content instanceof State) {
				State state = (State) content;
				System.out.println(state.getName());
				
				if (state.getName() == "") {
					System.out.println("Névtelen állapot megtalálva!");
					System.out.println("Adjon nevet az állapotnak!");
					System.out.println("Javasolt név: NévtelenÁllapot#" + nev);
					nev++;
				}
				
				if (state.getOutgoingTransitions().size() == 0) {
					System.out.println("A " + state.getName() + " állapot csapda állapot!");
					}
				else {
					for (Transition transition : state.getOutgoingTransitions()) {
					State state2 = (State) transition.getTarget();
					System.out.println(state.getName() + " -> " + state2.getName());
					}
				}
			}
		}*/
		
	
		
		// Transforming the model into a graph representation
		String content = model2gml.transform(root);
		// and saving it
		manager.saveFile("model_output/graph.gml", content);
	}
}
